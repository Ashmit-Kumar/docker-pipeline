pipeline {
    agent {label 'agent-1'}

    environment {
        DOCKER_REGISTRY = 'docker.io'  // Docker registry (can be Docker Hub or another registry)
        IMAGE_1_NAME = 'ashmit1020/blog-frontend:v1.3'  // Frontend image name
        IMAGE_2_NAME = 'ashmit1020/blog-backend:v1'  // Backend image name
	DOCKER_USER = 'ashmit1020'  // Replace with your Docker Hub username
 }

    stages {
	stage('Docker Login') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker-pat', variable: 'DOCKER_TOKEN')]) {
                        // Use the Docker Personal Access Token securely
                        sh "echo \$DOCKER_TOKEN | docker login -u ${DOCKER_USER} --password-stdin"
                    }
                }
            }
        }
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
                    sh "sudo docker pull ${IMAGE_1_NAME}"
                    sh "sudo docker pull ${IMAGE_2_NAME}"
                }
            }
        }

        stage('Run Docker Compose') {
            steps {
                script {
                    // Run the docker-compose command to start up the services
                    sh 'sudo docker-compose -f docker-compose.yml up -d'  // Use '-d' for detached mode
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    // Run your tests or checks here
                    // For example, checking if the services are running
                    sh 'sudo docker ps'  // List all running containers to confirm the services are up
                }
            }
         }
    }
}
