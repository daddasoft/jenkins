pipeline {
    agent any

    environment {
        // Define your API endpoint and default profile.
        API_URL = "https://yourapi.example.com/notify"
        PROFILE = "DEV"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from SCM
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Retrieve commit id and branch name using Git commands.
                    // The commit will be the short version of the current commit.
                    def commitName = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    // Attempt to get the branch name from GIT_BRANCH environment variable or via Git command.
                    def branchName = env.GIT_BRANCH ?: sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                    echo "Building commit: ${commitName} on branch: ${branchName}"
                }
                
                // Insert your build steps here
                sh 'echo "Executing build tasks..."'
            }
        }
    }

    post {
        success {
            script {
                // Retrieve commit and branch information again. Adjust if caching is appropriate.
                def commitName = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                def branchName = env.GIT_BRANCH ?: sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                echo "Build succeeded - notifying API."

                // Sending HTTPS request using curl on success.
                sh """
                    curl -X POST -H "Content-Type: application/json" \
                    -d '{ "status": "success", "commit": "${commitName}", "branch": "${branchName}", "profile": "${PROFILE}" }' \
                    ${API_URL}
                """
            }
        }
        failure {
            script {
                // Retrieve commit and branch information again.
                def commitName = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                def branchName = env.GIT_BRANCH ?: sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                echo "Build failed - notifying API."

                // Sending HTTPS request using curl on failure.
                sh """
                    curl -X POST -H "Content-Type: application/json" \
                    -d '{ "status": "failure", "commit": "${commitName}", "branch": "${branchName}", "profile": "${PROFILE}" }' \
                    ${API_URL}
                """
            }
        }
    }
}
