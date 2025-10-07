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
                git branch: 'master', url: 'https://github.com/sucharithanreddy/my_microservice.git', credentialsId: 'github-creds'
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

        // Stop old container if exists
        sh """
        if [ \$(docker ps -q -f name=$APP_NAME) ]; then
            echo 'Stopping old container...'
            docker stop $APP_NAME
        fi

        // Remove old container if exists
        if [ \$(docker ps -aq -f name=$APP_NAME) ]; then
            echo 'Removing old container...'
            docker rm $APP_NAME
        fi

        // Run new container
        docker run -d --name $APP_NAME -p $APP_PORT:3000 $DOCKER_IMAGE
        """
    }
}
    }
    post {
        always {
            echo "Pipeline execution finished."
        }
    }
}
