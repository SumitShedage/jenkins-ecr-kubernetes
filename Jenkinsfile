pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "720226180820"
        AWS_REGION = "ap-south-1"   
        ECR_REPO = "nginx-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Ritzz16/nginx-jenkins-ecr-minikube.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """
                aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/d7z7x8e8
                docker build -t nginx-app .
                docker tag nginx-app:latest public.ecr.aws/d7z7x8e8/nginx-app:latest
                """
            }
        }
        stage('Push Image to ECR') {
            steps {
                sh "docker push public.ecr.aws/d7z7x8e8/nginx-app:latest"
            }
        }
        stage('Deploy to Minikube') {
            steps {
                sh """
                minikube kubectl apply -f deployment.yaml
                minikube kubectl get pods 
                """
            }
        }
    }
}
