pipeline {
    agent any

    environment {
        ECR_REPO = 'shopfront'
        IMAGE_TAG = "latest"
        AWS_REGION = 'ap-southeast-2'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vinodkushwaha1996/docker-kubernetes-java-project.git', credentialsId: 'your-credentials-id'         }
        }

        stage('Compile') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
        stage('Copy Artifact') {
            steps {
                script {
                    sh 'cp target/*.jar .'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh '''
                    $(aws ecr get-login --no-include-email --region $AWS_REGION)
                    docker tag $ECR_REPO:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
                    docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }
    }
}

