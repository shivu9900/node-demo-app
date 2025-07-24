pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '237458753027.dkr.ecr.us-east-1.amazonaws.com/node-app'  // ✅ Use actual ECR repo URI
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/shivu9900/node-demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t node-app:${IMAGE_TAG} .'
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO'
                }
            }
        }

        stage('Tag & Push to ECR') {
            steps {
                script {
                    sh '''
                        docker tag node-app:${IMAGE_TAG} $ECR_REPO:${IMAGE_TAG}
                        docker push $ECR_REPO:${IMAGE_TAG}
                    '''
                }
            }
        }
    }
}
