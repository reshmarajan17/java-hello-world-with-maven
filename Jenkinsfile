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
              script {
              sh """
              mvn clean verify sonar:sonar
              """
              }
         }
       }
    }
}
