pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = 'sammed18' // Make sure this matches the Jenkins credentials ID
        DOCKER_IMAGE = 'sammed18/node-docker-app' // Remove the extra space
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/sammed-18/final_jen.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with a tag based on the build number
                    dockerImage = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                    echo "Docker image built: ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub using the provided credentials
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push("${BUILD_NUMBER}") // Push the image with the build number as the tag
                        echo "Docker image pushed to Docker Hub: ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('Clean Up') {
            steps {
                script {
                    echo "Attempting to remove Docker image: ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    try {
                        // Remove the locally built Docker image to clean up space
                        sh "docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                        echo "Docker image ${DOCKER_IMAGE}:${BUILD_NUMBER} removed successfully."
                    } catch (Exception e) {
                        echo "Failed to remove Docker image: ${e}"
                    }
                }
            }
        }
    }
    post {
        always {
            // Clean up workspace after the pipeline run
            cleanWs()
        }
    }
}
