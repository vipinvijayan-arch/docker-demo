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
                sh 'docker build -t docker-demo:${BUILD_NUMBER} .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop docker-demo || true
                    docker rm docker-demo || true

                    docker run -d \
                        --name docker-demo \
                        -p 8080:80 \
                        docker-demo:${BUILD_NUMBER}
                '''
            }
        }

        stage('Test') {
            steps {
                sh 'sleep 3'
                sh 'curl -f http://localhost:8080/'
            }
        }
    }
}
