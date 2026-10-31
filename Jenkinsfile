
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins-docker-demo'
        CONTAINER_NAME = 'jenkins-demo'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Checking Docker image...'
                sh 'docker image inspect ${IMAGE_NAME}:latest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 8081:80 \
                    ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
