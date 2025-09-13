pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "767398125076"
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
                aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/w4c1w9k0
                docker build -t nginx-app .
                docker tag nginx-app:latest public.ecr.aws/w4c1w9k0/nginx-app:latest
                """
            }
        }
        stage('Push Image to ECR') {
            steps {
                sh "docker push public.ecr.aws/w4c1w9k0/nginx-app:latest"
            }
        }
        stage('Deploy to Minikube') {
            steps {
                sh """
                kubectl apply -f deployment.yaml
                kubectl get pods 
                """
            }
        }
    }
}

