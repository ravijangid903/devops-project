pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t devops-demo .'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 3000:3000 --name devops-demo devops-demo'
            }
        }
    }
}
