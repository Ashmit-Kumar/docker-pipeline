pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'docker.io'  // Docker registry (can be Docker Hub or another registry)
        IMAGE_1_NAME = 'ashmit1020/blog-frontend:v1.2'  // Frontend image name
        IMAGE_2_NAME = 'ashmit1020/blog-backend:v1'  // Backend image name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the GitHub repository
                git branch: 'main', url: 'https://github.com/Ashmit-Kumar/docker-pipeline'
            }
        }

        stage('Set up Docker Compose') {
            steps {
                script {
                    // Make sure Docker Compose is available
                    sh 'docker-compose --version'
                    
                    // Pull the pre-built images from Docker registry (if needed)
                    // This step ensures that the images are pulled before we run them.
                    sh "docker pull ${IMAGE_1_NAME}"
                    sh "docker pull ${IMAGE_2_NAME}"
                }
            }
        }

        stage('Run Docker Compose') {
            steps {
                script {
                    // Run the docker-compose command to start up the services
                    sh 'docker-compose -f docker-compose.yml up -d'  // Use '-d' for detached mode
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    // Run your tests or checks here
                    // For example, checking if the services are running
                    sh 'docker ps'  // List all running containers to confirm the services are up
                }
            }
        }

        stage('Tear Down') {
            steps {
                script {
                    // Optionally, shut down the services after tests
                    sh 'docker-compose down'
                }
            }
        }
    }
}
