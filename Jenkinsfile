pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-webapp:latest .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker rm -f sample-webapp || true
                    docker run -d \
                      --name sample-webapp \
                      --restart unless-stopped \
                      -p 8081:80 \
                      sample-webapp:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'curl --fail --silent http://localhost:8081'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed. Check the Console Output.'
        }
    }
}
