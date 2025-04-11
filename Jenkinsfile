pipeline {
    agent any
 

    environment {
        MY_CUSTOM_VAR = "customValue"
        COMMIT_MESSAGE = ""
    }

    parameters {
        string(name: 'MY_PARAM', defaultValue: 'default', description: 'A build parameter')
        string(name: 'PROFILE', defaultValue: 'DEV', description: 'Profile to build')
    }



    stages {




        stage('testing Message Sending') {
            steps {
               script {
                    sendFormattedSlackNotification('SUCCESS', params.PROFILE)
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
                echo "GIT_COMMIT is ${env.GIT_COMMIT}"
                echo "GIT_BRANCH is ${env.GIT_BRANCH}"
                echo "GIT_URL is ${env.GIT_URL}"
                echo "GIT_TAG is ${env.GIT_TAG}"
                echo "GIT_CHANGELOG is ${env.GIT_CHANGELOG}"
                echo "GIT_AUTHOR is ${env.GIT_AUTHOR}"
                echo "GIT_AUTHOR_EMAIL is ${env.GIT_AUTHOR_EMAIL}"
                echo "Commit message is ${env.GIT_COMMIT_MESSAGE}"
                echo "change title name is ${env.CHANGE_TITLE}"
                echo "change id is ${env.CHANGE_ID}"
                echo "change target is ${env.CHANGE_TARGET}"
                echo "build tag ${env.BUILD_TAG}"
                echo "build display name ${env.GIT_PREVIOUS_COMMIT}"
                echo "build display name ${env.GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
                echo "build display name ${env.SVN_REVISION}"
                // get commit message by hash
                // Use shell command on Ubuntu
                sh 'echo "Checking out..."' 

            }
        }

    stage('Test') {
    steps {
        checkout scm
        script {
            COMMIT_MESSAGE = getCommitMessage()
            echo "Commit message: ${COMMIT_MESSAGE}"
            def commitHash = getHash()
            echo "Commit hash: ${commitHash}"
        }
    }
}
        stage('Build') {
            steps {
                echo "Building on branch: ${env.BRANCH_NAME}"
                // Use shell command on Ubuntu
                 
            }
        }
    }

    post {
        always {
             cleanWs()
        }
        success {
            script {
                                  sendFormattedSlackNotification('SUCCESS', params.PROFILE)

            }
        }
        failure {
            script {
                                 sendFormattedSlackNotification('SUCCESS', params.PROFILE)

            }
        }
    }
}

// Function to get commit message
String getCommitMessage(String commitHash = env.GIT_COMMIT) {
    cmd = "git log -1 --format=\"%B\" ${commitHash}"
    output = sh(script: cmd, returnStdout: true).trim()
    return output.trim()
}

String getHash() {
    cmd = 'git rev-parse --short HEAD'
    output = sh(script: cmd, returnStdout: true).trim()
    return output.trim()
}

void sendFormattedSlackNotification(String status, String profile) {
    // Set emoji based on status
    emoji = status.equalsIgnoreCase("success") ? ":white_check_mark:" : ":x:"
    
    message = """Build ${status.toLowerCase()} for Winpharm Backend  *${profile}/${getHash()}/\\"${getCommitMessage()}\\"* """
    
    withCredentials([string(credentialsId: 'SLACK_HOOK', variable: 'SLACK_WEBHOOK_URL')]) {
        sh """
            curl -X POST "\${SLACK_WEBHOOK_URL}" \\
            -H "Content-Type: application/json" \\
            -d '{"text": "${emoji} ${message}"}'
        """
    }
}