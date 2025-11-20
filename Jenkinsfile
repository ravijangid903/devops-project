pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-app:latest .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag devops-app:latest ravijangid903/devops-app:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', 
                                                  usernameVariable: 'USER', 
                                                  passwordVariable: 'PASS')]) {
                    sh """
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push ravijangid903/devops-app:latest
                    docker logout
                    """
                }
            }
        }

        stage('Remove Old Container') {
            steps {
                sh """
                if [ \$(docker ps -aq -f name=devops-app) ]; then
                    docker rm -f devops-app || true
                fi
                """
            }
        }

        stage('Deploy New Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name devops-app ravijangid903/devops-app:latest'
            }
        }
    }

    post {
        success {
            echo '🎉 Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
