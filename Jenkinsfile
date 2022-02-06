pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'echo "test"'
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
