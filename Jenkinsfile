pipeline {
    agent any
    
    environment {
        APP_NAME = "reddit-clone-pipe"
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
                git branch: 'main', url: 'https://github.com/NehaKyatham/a-reddit-clone-gitops'
            }
        }
        
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "nehakyatham"
                    git config --global user.email "nehakyatham@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                    git push https://github.com/NehaKyatham/a-reddit-clone-gitops main
                """
            }
        }
    }
}

