pipeline {
    agent any
    
      environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "18.237.236.190:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "test-maven-poc"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "f539982c-b0c5-404e-bf60-5b30e75c1055"
    }
    stages {
        stage('build') {
            steps {
                sh """
                which mvn
                mvn clean package
                """
            }
        }
        stage('test') {
          steps {
              withSonarQubeEnv(credentialsId: '355ff75d-92e3-4669-bb84-b42488c4341a', installationName: 'sonar_docker_installation') { // You can override the credential to be used
              sh 'mvn verify sonar:sonar -Dsonar.projectKey=poc_deployment'
              } 
          }
        }
        stage('nexus') {
          steps {
              script {
                   pom = readMavenPom file: "pom.xml";
                    // Find built artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    // Print some info from the artifact found
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    // Extract the path from the File found
                    artifactPath = filesByGlob[0].path;
                    // Assign to a boolean response verifying If the artifact name exists
                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],

                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );

                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
              }
              }
         }
       }
    }
