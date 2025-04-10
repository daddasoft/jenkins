pipeline {
    agent any
 

    environment {
        MY_CUSTOM_VAR = "customValue"
        COMMIT_MESSAGE = ""
    }

    parameters {
        string(name: 'MY_PARAM', defaultValue: 'default', description: 'A build parameter')
    }



    stages {

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
                // Use batch command on Windows
                bat 'echo "Checking out..."'
            }
        }

    stage('Test') {
    steps {
        checkout scm
        script {
            COMMIT_MESSAGE = getCommitMessage()
            echo "Commit message: ${COMMIT_MESSAGE}"
        }
    }
}
        stage('Build') {
            steps {
                echo "Building on branch: ${env.BRANCH_NAME}"
                // Use batch command on Windows
                bat 'echo "Building..."'
            }
        }
    }

    post {
        success {
            script {
                echo "Build succeeded!"
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Custom Variable: ${env.MY_CUSTOM_VAR}"
                echo "Workspace: ${env.WORKSPACE}"
                echo "Node Name: ${env.NODE_NAME}"
                echo "Build URL: ${env.BUILD_URL}"
                echo "Git Commit: ${env.GIT_COMMIT}"
                echo "Git Branch: ${env.GIT_BRANCH}"
                echo "Git URL: ${env.GIT_URL}"
                echo "Git Tag: ${env.GIT_TAG}"
                echo "Git Change Log: ${env.GIT_CHANGELOG}"
                echo "Git Author: ${env.GIT_AUTHOR}"
                echo "Git Author Email: ${env.GIT_AUTHOR_EMAIL}"
                echo "Build result: ${currentBuild.result}"
                echo "Build duration: ${currentBuild.duration}ms"
                if (params.MY_PARAM) {
                    echo "Parameter MY_PARAM: ${params.MY_PARAM}"
                }
                bat "echo \"Build completed successfully! ${params.MY_PARAM}   ${env.GIT_COMMIT}   on Commit  \""
                sendSlackNotification(":check_mark: SUCCESS", "Build succeeded for ${env.JOB_NAME} #${env.BUILD_NUMBER} on branch ${env.GIT_BRANCH}. Commit message: ${COMMIT_MESSAGE}")
            }
        }
        failure {
            script {
                echo "Build failed!"
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                 echo "Build result: ${currentBuild.result}"
                 echo "Build duration: ${currentBuild.duration}ms"
                // Additional failure handling can also use global variables.
            }
        }
    }
}

// Function to get commit message
def getCommitMessage(commitHash = env.GIT_COMMIT) {
    def cmd = "git log -1 --format=\"%%B\" ${commitHash}"
    echo "Running command: ${cmd}"
    def output = bat(script: cmd, returnStdout: true).trim()
    
    // Extract actual commit message from bat output
    // On Windows, the output typically includes the command echo and other text
    return output.readLines().drop(1).join("\n").trim()
}

// Function to send Slack notification
def sendSlackNotification(String status, String message = null) {

     def slackWebhookUrl = "https://hooks.slack.com/services/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"

    def payload = [
        text: "${status}: ${message}",
      ]

    def jsonPayload = groovy.json.JsonOutput.toJson(payload).replace('"', '\\"')

    bat "curl -X POST -v -H \"Content-type: application/json\" --data \"${jsonPayload}\" ${slackWebhookUrl}"
}
