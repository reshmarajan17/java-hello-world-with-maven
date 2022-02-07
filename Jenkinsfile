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
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: "org.springframework",
                            version: "0.1.0",
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: "jb-hello-world-maven",
                                classifier: '',
                                file: "test-maven-poc/org/springframework/jb-hello-world-maven/0.1.0/jb-hello-world-maven-0.1.0.jar.",
                                type: "jar"],

                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );

                   
              }
              }
         }
       }
    }
