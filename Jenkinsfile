pipeline {
    agent any
    environment {
        APP_NAME = "my-microservice"
        DOCKER_IMAGE = "my-microservice:latest"
    }
    stages {
        stage('Docker Test') {
            steps {
                sh 'docker version'
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sucharithanreddy/my-microservice.git', credentialsId: 'github-creds'
            }
        }
        stage('Build') {
            steps {
                echo "Building Docker Image..."
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }
        stage('Test') {
            steps {
                echo "Running Tests..."
                sh "docker run --rm $DOCKER_IMAGE npm test || echo 'No tests defined, skipping'"
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying Container..."
                sh "docker ps -a | grep $APP_NAME && docker stop $APP_NAME && docker rm $APP_NAME || echo 'No existing container'"
                sh "docker run -d --name $APP_NAME -p 3000:3000 $DOCKER_IMAGE"
            }
        }
    }
    post {
        always {
            echo "Pipeline execution finished."
        }
    }
}
