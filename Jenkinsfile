pipeline {

    // ✅ Try agent first, fallback to any if needed
    agent {
        label 'agent'
    }

    environment {
        APP_NAME = "carwebsite"
        CONTAINER_NAME = "carwebsite_container"
        PORT = "80"
    }

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout') {
            steps {
                echo "📥 Cloning latest code..."
                checkout scm
            }
        }

        stage('Check Files') {
            steps {
                echo "📂 Listing project files..."
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh 'docker build -t $APP_NAME:latest .'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo "🛑 Stopping old container..."
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                echo "🚀 Starting new container..."
                sh '''
                docker run -d -p ${PORT}:80 --restart always \
                --name $CONTAINER_NAME $APP_NAME:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "✅ Verifying running containers..."
                sh 'docker ps'
            }
        }

        stage('Cleanup') {
            steps {
                echo "🧹 Cleaning unused Docker images..."
                sh 'docker system prune -f'
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
        always {
            echo '📦 Pipeline execution finished'
        }
    }
}