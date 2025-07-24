pipeline {
    agent any
    environment {
        APP_SERVER = "ubuntu@10.100.3.135"  // Replace with actual IP
        IMAGE_NAME = "node-demo-app"
        REPO_URL = "237458753027.dkr.ecr.us-east-1.amazonaws.com"
    }
    stages {
        stage('Build & Push from App Server') {
            steps {
                sshagent(['app-server-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $APP_SERVER '
                            cd ~/node-demo-app &&
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $REPO_URL &&
                            docker build -t $IMAGE_NAME:latest . &&
                            docker tag $IMAGE_NAME:latest $REPO_URL/$IMAGE_NAME:latest &&
                            docker push $REPO_URL/$IMAGE_NAME:latest
                        '
                    """
                }
            }
        }
    }
}
