def inputEnv = ""
def inputOperation = ""

pipeline {
    agent {
        label 'pi'
    }
    environment {
       VERSION = '1.0.4'
       HUBOT_DEFAULT_ROOM = 'jenkins'
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
                            inputEnv = hubotApprove message: 'Which environment do you want to apply to?', ok: "Proceed", id: "InputEnv", submitterParameter: 'submitter',
                            parameters: [
                                    choice(name: 'inputEnv', choices: 'test\nuat\nprod\nAbort', description: "'test', 'uat' or 'prod'")
                                ]  
                        }
                        echo "inputEnv: ${inputEnv.inputEnv}"
                    } catch (err) {
                        echo "Didn't obtain hubot inputEnv before timeout!"
                    }

                    if (inputEnv.inputEnv == 'Abort') {
                        currentBuild.result = 'ABORTED'
                        error("Build has been aborted by @${inputEnv.submitter}")
                    }

                    try {
                        timeout (time: 15, unit: 'MINUTES') {
                            inputOperation = hubotApprove message: 'Do you want to deploy or destroy a stack?', ok: 'Proceed', id: 'InputOperation', submitterParameter: 'submitter',
                            parameters: [
                                choice(name: 'inputOperation', choices: 'Deploy\nDestroy\nAbort', description: 'deploy a new stack, or destroy an existed stack')
                            ]
                        }
                        echo "inputOperation: ${inputOperation.inputOperation}"
                    } catch (err) {
                        echo "Didn't obtain user inputOperation before timeout!"
                    }

                    if (inputOperation.inputOperation == 'Abort') {
                        currentBuild.result = 'ABORTED'
                        error("Build has been aborted by @${inputOperation.submitter}")
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

    post {
        success {
            script {
                hubotSend message: "Build finished successfully!" 
            }
        }
        aborted {
            script {
                hubotSend message: "Build has been aborted."
            }
        }
        failure {
            script {
                hubotSend message: "Build failed."
            }
        }
    }
}
