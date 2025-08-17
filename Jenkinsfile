pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "latest" // Change this manually or make it dynamic
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
                    credentialsId: 'github',
                    url: 'https://github.com/Anuragsaroj/project_1_gitops-register-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    echo "[INFO] Current deployment.yaml:"
                    cat deployment.yaml

                    echo "[INFO] Updating image tag to: ${APP_NAME}:${IMAGE_TAG}"
                    sed -i 's|image:.*|image: ${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml

                    echo "[INFO] Updated deployment.yaml:"
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "Anuragsaroj"
                    git config --global user.email "anuragsaroj0008@gmail.com"
                    git add deployment.yaml
                    git diff --cached --quiet || git commit -m "Updated Deployment Manifest"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh """
                        git push https://Anuragsaroj:${GIT_PASSWORD}@github.com/Anuragsaroj/project_1_gitops-register-app.git main
                    """
                }
            }
        }
    }
}
