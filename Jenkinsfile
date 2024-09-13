pipeline {
    agent any
    environment {

        // Define environment variables
        AWS_DEFAULT_REGION = 'us-east-2' //This will stay the same since your AWS_DEFAULT_REGION name is the same 
        AWS_ACCOUNT_ID = '296062590937' //change this to your AWS_ACCOUNT_ID
        ECR_REPOSITORY = 'my-application-repo' //This will stay the same since your ECR_REPOSITORY name is the same 
        FRONTEND_SERVICE_NAME = 'frontend-service' //This will stay the same since your FRONTEND_SERVICE_NAME name is the same
        BACKEND_SERVICE_NAME = 'backend-service'  //This will stay the same since your BACKEND_SERVICE_NAME name is the same
        ECS_CLUSTER_NAME = 'my-cluster' //This will stay the same since your ECS_CLUSTER_NAME name is the same
    }
    stages {
        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker images for frontend and backend
                    sh 'docker build -t $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$ECR_REPOSITORY:frontend ./frontend'
                    sh 'docker build -t $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$ECR_REPOSITORY:backend ./backend'
                }
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    // Login to ECR
                    sh 'aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com'
                    // Push images to ECR
                    sh 'docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$ECR_REPOSITORY:frontend'
                    sh 'docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$ECR_REPOSITORY:backend'
                }
            }
        }
        stage('Update ECS Services') {
            steps {
                script {
                    // Force a new deployment of the ECS services
                    sh "aws ecs update-service --cluster $ECS_CLUSTER_NAME --service $FRONTEND_SERVICE_NAME --force-new-deployment"
                    sh "aws ecs update-service --cluster $ECS_CLUSTER_NAME --service $BACKEND_SERVICE_NAME --force-new-deployment"
                }
            }
        }
    }
}
