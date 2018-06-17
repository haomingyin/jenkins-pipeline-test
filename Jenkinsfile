pipeline {
    agent {
        label 'master'
    }
    environment {
       VERSION = '1.0.3' 
    }
    stages {
        stage('Print Version') {
            steps {
                echo "Jenkinsfile version: ${env.VERSION}"
            }
        }
        
        stage('Env Checkup') {
            steps {
                sh "printenv"
            }
        }
        
        stage('Current Work Directory') {
            steps {
                sh "pwd"
            }
        }
        
        stage('User') {
            steps {
                sh "whoami"
            }
        }
    }
}
