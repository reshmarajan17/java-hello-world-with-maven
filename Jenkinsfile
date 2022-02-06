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
        stage('test_parameter') {
          steps {
              script {
              input message: 'enter region', parameters: [string(defaultValue: 'us-east-1', name: 'region')]
              sh """
              cat Jenkinsfile
              """
              }
         }
       }
    }
}
