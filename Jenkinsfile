pipeline {
    agent any

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
                // Simulate a build step
                sh 'echo "Building..."'
            }
        }
    }

    post {
        success {
            script {
                // Access global variables in the post block
                echo "Build succeeded!"
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Custom Variable: ${env.MY_CUSTOM_VAR}"
                if (params.MY_PARAM) {
                    echo "Parameter MY_PARAM: ${params.MY_PARAM}"
                }
            }
        }
        failure {
            script {
                echo "Build failed!"
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                // Additional failure handling can also use global variables.
            }
        }
    }
}
