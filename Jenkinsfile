pipeline {
    agent any
    environment {
        APP_NAME = "reddit-clone-app"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/HamzaMerdassi55/Gitops-clone'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   echo "Original deployment.yaml:"
                   cat deployment.yaml
                   sed -i 's#hamza6/reddit-clone-pipeline:.*#hamza6/reddit-clone-pipeline:${IMAGE_TAG}#g' deployment.yaml
                   echo "Updated deployment.yaml:"
                   cat deployment.yaml
                """
            }
        }
        stage("Push the Changed Deployment File to GitHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                        git config --global user.name "HamzaMerdassi55"
                        git config --global user.email "hamzamerdassi41@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest with new image tag"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/HamzaMerdassi55/Gitops-clone.git main
                    """
                }
            }
        }
    }
}
