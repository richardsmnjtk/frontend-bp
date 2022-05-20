pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "180602451697"
        AWS_DEFAULT_REGION = "ap-southeast-1"
        IMAGE_REPO_NAME = "frontend-bp"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages {
        // Login To AWS ECR
        stage('Logging into AWS ECR') {
            steps {
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com" 
            }
        }

        // Building Docker images
        stage('Build') {
            steps {
                    sh "docker build -t ${IMAGE_REPO_NAME} ./frontend/" //if Dockerfile present same as Jenkins use this >> docker build -t ${IMAGE_REPO_NAME} .
                }
            }

        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps {
                    sh "docker tag ${IMAGE_REPO_NAME}:latest ${REPOSITORY_URI}:latest"
                    sh "docker push ${REPOSITORY_URI}:latest"
            }
        }
    }
}