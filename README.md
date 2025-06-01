PROG10

trigger:
  - main

pool:
  name: 'MyLocalPool'  # Your self-hosted agent pool name

steps:
  # Step 1: Checkout the Code from GitHub
  - checkout: self
    displayName: 'Checkout Code from GitHub'

  # Step 2: Build and Run Unit Tests
  - script: mvn clean test
    displayName: 'Build and Run Unit Tests'

  # Step 3: Publish Test Results (JUnit)
  - task: PublishTestResults@2
    inputs:
      testResultsFiles: '**/target/surefire-reports/TEST-*.xml'
      testResultsFormat: 'JUnit'
      failTaskOnMissingResultsFile: true
    displayName: 'Publish Maven Test Results'

  # Step 4: Publish Build Artifacts
  - task: PublishBuildArtifacts@1
    inputs:
      PathtoPublish: 'target'
      ArtifactName: 'drop'
      publishLocation: 'Container'
    displayName: 'Publish Build Artifacts'



    PROG11

    trigger:
  - main

pool:
  name: MYLOCALPOOL  # Name of your self-hosted agent pool

steps:
  - task: Maven@3
    inputs:
      mavenPomFile: 'pom.xml'
      goals: 'package'

  - script: |
      echo "Simulating deployment..."
      mkdir deployed
      copy target\*.jar deployed\
      echo "Deployment successful!"
    displayName: 'Simulate Deployment'

  - script: |
      echo "Contents of deployed folder:"
      dir deployed
    displayName: 'Verify Deployment Output'


PROG12

<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-jar-plugin</artifactId>
      <version>3.2.2</version>
      <configuration>
        <archive>
          <manifest>
            <addClasspath>true</addClasspath>
            <mainClass>com.yourpackage.HelloWorld</mainClass>
          </manifest>
        </archive>
      </configuration>
    </plugin>
  </plugins>
</build>





trigger:
  - main

pool:
  name: MYLOCALPOOL

steps:
  - task: Maven@3
    inputs:
      mavenPomFile: 'pom.xml'
      goals: 'clean package'

  - script: |
      echo "Simulating deployment..."
      mkdir deployed
      copy target\*.jar deployed\
      echo "Deployment successful!"
    displayName: 'Simulate Deployment'

  - script: |
      echo "Running the JAR file..."
      java -jar deployed\*.jar
    displayName: 'Run Application'

