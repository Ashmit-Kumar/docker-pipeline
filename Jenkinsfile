pipeline {
    agent any

    environment {
        // Define your Docker images
        IMAGE_1_NAME = 'blog'
        IMAGE_2_NAME = 'blog-api'
        DOCKER_REGISTRY = 'docker.io' // Optional if you want to push to a registry
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the GitHub repository
                git branch: 'main', url: 'https://github.com/Ashmit-Kumar/docker-pipeline'
            }
        }

        stage('Build Image 1') {
            steps {
                script {
                    // Build Docker image 1
                    docker.build("${IMAGE_1_NAME}:${BUILD_ID}")
                }
            }
        }

        stage('Build Image 2') {
            steps {
                script {
                    // Build Docker image 2
                    docker.build("${IMAGE_2_NAME}:${BUILD_ID}")
                }
            }
        }

        stage('Push Images') {
            steps {
                script {
                    // Push images to Docker registry (if required)
                    docker.withRegistry('https://${DOCKER_REGISTRY}', 'docker-hub-pat-credentials') {
                        docker.image("${IMAGE_1_NAME}:${BUILD_ID}").push()
                        docker.image("${IMAGE_2_NAME}:${BUILD_ID}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images after the build
            sh 'docker system prune -f'
        }
    }
}
