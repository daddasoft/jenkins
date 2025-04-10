pipeline {
    agent any

    node {
    withCheckout(scm) {
         echo "GIT_COMMIT is ${env.GIT_COMMIT}"
    }
}

    environment {
        MY_CUSTOM_VAR = "customValue"
    }

    parameters {
        string(name: 'MY_PARAM', defaultValue: 'default', description: 'A build parameter')
    }

    stages {
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
