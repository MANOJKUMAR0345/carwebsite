pipeline {
    agent { label 'agent' }   // run on agent (not controller)

    environment {
        APP_NAME = "carwebsite"
        CONTAINER_NAME = "carwebsite_container"
        PORT = "80"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Files') {
            steps {
                sh 'echo "Listing project files..."'
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building Docker image..."
                docker build -t $APP_NAME:latest .
                '''
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh '''
                echo "Stopping old container (if exists)..."
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                echo "Starting new container..."
                docker run -d -p ${PORT}:80 --name $CONTAINER_NAME $APP_NAME:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                echo "Running containers:"
                docker ps
                '''
            }
        }

        stage('Cleanup') {
            steps {
                sh '''
                echo "Cleaning unused Docker images..."
                docker system prune -f
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
