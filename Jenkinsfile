
pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ACCOUNT_ID = '329195319784'
        REPO_NAME = 'my-app'
        IMAGE = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${REPO_NAME}:latest"
    }

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag my-app:latest $IMAGE'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE'
            }
        }
    }
}
