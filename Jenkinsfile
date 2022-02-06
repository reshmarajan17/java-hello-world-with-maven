pipeline {
    agent any
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
              sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:4.6.2.2472:sonar'
              }
         }
       }
    }
}
