env.DOCKER_REGISTRY = '180602451697.dkr.ecr.ap-southeast-1.amazonaws.com'
env.DOCKER_IMAGE_NAME = 'frontend-bp'
pipeline {
    agent any
    stages {
        stage('Git Pull from Github') {
            steps {
                git url: 'https://github.com/richardsmnjtk/frontend-bp.git'
            }
        }    
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
            }
        }              
        stage('Push Docker Image to ECR') {
            steps {
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
            }
        } 
        stage('Deploy To Kubernetes Cluster') {
            steps {
                sh'''sed -i "15d" p-frontend-deployment.yaml'''
                sh'''sed -i "14 a \'\\'          image: $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}" frontend.yaml && sed -i "s/''//" frontend.yaml'''
                sh "kubectl apply -f frontend.yaml"                
            }
        }
        stage('Remove Docker Image in local') {
            steps {
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
            }
        }
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }        
    }