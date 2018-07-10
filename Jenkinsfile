def userInput = ""

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

        stage('User Input') {
            steps {
                script {
                    try {
                        timeout (time: 15, unit: "MINUTES") {
                            userInput = input message: 'Which opertion do you want to apply', ok: 'Proceed', id: testUserInput,
                            parameters: [
                                choice(name: 'operationType', choices: 'deploy\ndestroy', description: 'Deploy or destroy?')
                            ]
                        }
                        echo "User input: ${userInput}"
                    } catch (err) {
                        echo "Didn't obtain user input before timeout!"
                    }
                }
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
