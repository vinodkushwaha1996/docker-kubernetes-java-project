pipeline {
    agent any

    environment {
        ECR_REPO = 'shopfront'
        IMAGE_TAG = "latest"
        AWS_REGION = 'ap-southeast-2'
    }
    tools {
        maven 'Maven 3.9.9' // Use the name you provided in the Global Tool Configuration
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vinodkushwaha1996/docker-kubernetes-java-project.git', credentialsId: '4c70651a-ed30-42ef-83bf-ad8244981573'         }
        }
        stage('Set Up Maven') {
            steps {
                sh 'export PATH=$PATH:/opt/apache-maven-3.9.9/bin'
            }
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

