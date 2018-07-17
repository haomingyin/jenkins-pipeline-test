def userInput1 = ""
def userInput2 = ""

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
                        timeout (time: 15, unit: 'MINUTES') { 
                            userInput = hubotApprove message: 'Make a choice ;)', tokens: "BUILD_NUMBER,BUILD_ID", submitterParameter: "submiiter", ok: "Confirm", id: "HubotInput1",
                            parameters: [
                                    choice(name: 'hubotInput1', choices: 'choice1-1\nchoice1-2\nchoice1-3', description: '1, 2, or 3')
                                ]  
                        }
                        echo "Hubot input 1: ${userInput}"
                        echo "Hubot input 1 submitter: ${userInput.submitter}"
                    } catch (err) {
                        echo "Didn't obtain hubot input 1 before timeout!"
                    }

                    try {
                        timeout (time: 15, unit: 'MINUTES') {
                            userInput = input message: 'Which option do you want to choose', ok: 'Proceed', id: 'TestUserInput1', submitterParameter: 'submitter',
                            parameters: [
                                choice(name: 'userInput1', choices: 'choice1-1\nchoice1-2\nchoice1-3', description: '1, 2, or 3')
                            ]
                        }
                        echo "User input 1: ${userInput}"
                        echo "User input 1 submitter: ${userInput.submitter}"
                    } catch (err) {
                        echo "Didn't obtain user input 1 before timeout!"
                    }

                    try {
                        timeout (time: 15, unit: 'MINUTES') {
                            userInput = input message: 'Which option do you want to choose', ok: 'Proceed', id: 'TestUserInput2',
                            parameters: [
                                choice(name: 'userInput2', choices: 'choice2-1\nchoice2-2\nchoice2-3', description: '1, 2 or 3'),
                                booleanParam(name: 'userInput3', description: 'Are you happy or not'),
                                text(name: 'userInput4', description: 'Please enter some random things')
                            ]
                        }
                        echo "User input 2: ${userInput}"
                    } catch (err) {
                        echo "Didn't obtain user input 2 before timeout!"
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
