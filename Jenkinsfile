pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_REGION = 'us-east-1'  // Change to your AWS region
        AWS_ACCOUNT_ID = '962524581616'
        REPO_NAME = 'devops-capstone'
        IMAGE_TAG = 'latest'
        ECR_URL = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${REPO_NAME}"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ashakumar87/devops-capstone.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_URL:$IMAGE_TAG .'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin $ECR_URL
                '''
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                sh 'docker push $ECR_URL:$IMAGE_TAG'
            }
        }
    }
}
