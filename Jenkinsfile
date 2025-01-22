pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'  // Replace with your AWS region
        ECR_REPO_NAME = 'fastapi-app'
        IMAGE_TAG = 'latest'
        AWS_ACCOUNT_ID = '123456789012'  // Replace with your AWS account ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh """
                docker build -t ${ECR_REPO_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Tag Docker Image') {
            steps {
                echo "Tagging Docker image..."
                sh """
                docker tag ${ECR_REPO_NAME}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Push to AWS ECR') {
            steps {
                echo "Pushing Docker image to AWS ECR..."
                sh """
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                """
            }
        }
    }
}