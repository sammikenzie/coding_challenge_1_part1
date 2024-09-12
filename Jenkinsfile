pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-2'  // Replace with your AWS region
        ECR_REPO = 'tech_challenge_1'  // Replace with your ECR repository name
        DOCKER_IMAGE_TAG = 'latest'  // Tag for Docker image
        AWS_ACCOUNT_ID = '296062590937'  // Replace with your AWS account ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Pulls the code from GitHub repository
                git branch: 'main', url: 'https://github.com/your-username/your-repo-name.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t $ECR_REPO:$DOCKER_IMAGE_TAG .'
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Log in to AWS ECR
                    sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com'

                    // Tag the image with the ECR repo URL
                    sh 'docker tag $ECR_REPO:$DOCKER_IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$DOCKER_IMAGE_TAG'

                    // Push the Docker image to AWS ECR
                    sh 'docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$DOCKER_IMAGE_TAG'
                }
            }
        }

        stage('Deploy to AWS ECS') {
            steps {
                script {
                    // Deploy Docker image to AWS ECS
                    sh 'aws ecs update-service --cluster your-cluster-name --service your-service-name --force-new-deployment'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
