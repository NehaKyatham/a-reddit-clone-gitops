pipeline {
    agent any

    environment {
        APP_NAME = "reddit-clone-pipeline"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {

        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/NehaKyatham/a-reddit-clone-gitops'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    echo "Before update:"
                    cat deployment.yaml

                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml

                    echo "After update:"
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "NehaKyatham"
                    git config --global user.email "nehakyatham@gmail.com"

                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github-token', gitToolName: 'Default')]) {
                    sh """
                        git push https://github.com/NehaKyatham/a-reddit-clone-gitops main
                    """
                }
            }
        }

    } // end stages
} // end pipeline

