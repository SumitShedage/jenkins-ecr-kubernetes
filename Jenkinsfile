pipeline {
    agent any
    environment {
        ECR_PUBLIC_REGISTRY = "public.ecr.aws/w4c1w9k0"
        ECR_REPO = "nginx-app"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            steps {
               git branch: 'main', url: 'https://github.com/SumitShedage/jenkins-ecr-kubernetes.git'
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
        stage('Deploy to k8s') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                    kubectl apply -f deployment.yaml
                    kubectl get pods
                    """
                }
            }
        }
    }
}
