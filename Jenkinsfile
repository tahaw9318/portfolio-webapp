pipeline {
    agent any

    environment {
        IMAGE_NAME = 'taha221016/portfolio-webapp'
        DOCKER_CREDENTIALS = 'dockerhub-creds' // Your Docker credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your project repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the current directory
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Log into Docker Hub using the stored credentials
                    withDockerRegistry([credentialsId: DOCKER_CREDENTIALS, url: 'https://index.docker.io/v1/']) {
                        // Push the image to Docker Hub
                        sh 'docker push $IMAGE_NAME'
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up any resources or containers if needed
            cleanWs()
        }
    }
}
