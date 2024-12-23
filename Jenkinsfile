pipeline {
    agent any
    
    environment {
        // Define Docker image name
        DOCKER_IMAGE = 'taha221016/portfolio-webapp'
        DOCKER_CREDENTIALS = 'dockerhub-creds'  // Set your Jenkins Docker credentials here
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the source code from your Git repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the repository
                    echo "Building Docker image: ${DOCKER_IMAGE}"
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using credentials stored in Jenkins
                    echo "Logging into Docker Hub"
                    withDockerRegistry([credentialsId: DOCKER_CREDENTIALS]) {
                        sh 'docker login'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the built Docker image to Docker Hub
                    echo "Pushing Docker image to Docker Hub"
                    withDockerRegistry([credentialsId: DOCKER_CREDENTIALS]) {
                        sh 'docker push ${DOCKER_IMAGE}'
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Clean up Docker images and containers after the build
                    echo "Cleaning up"
                    sh 'docker system prune -f'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and push successful!'
        }
        failure {
            echo 'Build or push failed!'
        }
        always {
            echo 'Cleaning up workspace'
            cleanWs()  // Clean up workspace after the build
        }
    }
}
