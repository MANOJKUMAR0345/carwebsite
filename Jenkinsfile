
pipeline {
    agent any

    stages {

       stage('Check Files') {
    steps {
        sh 'ls -la'
    }
}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t carwebsite:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop carwebsite_container || true
                docker rm carwebsite_container || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 80:80 --name carwebsite_container carwebsite:latest'
            }
        }
    }
}
