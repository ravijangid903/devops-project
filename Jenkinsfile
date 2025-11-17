pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling latest code from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh """
                docker build -t devops-app:latest .
                """
            }
        }

        stage('Remove Old Container') {
            steps {
                echo 'Stopping & removing old container if exists...'
                sh """
                if [ \$(docker ps -aq -f name=devops-app) ]; then
                    docker rm -f devops-app || true
                fi
                """
            }
        }

        stage('Run New Container') {
            steps {
                echo 'Deploying new container...'
                sh """
                docker run -d -p 3000:3000 --name devops-app devops-app:latest
                """
            }
        }
    }

    post {
        success {
            echo '🎉 Deployment Success! App is live on port 3000.'
        }
        failure {
            echo '❌ Deployment Failed. Please check console logs.'
        }
    }
}
