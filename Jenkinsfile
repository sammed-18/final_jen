pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = 'sammed18'
        DOCKER_IMAGE = ' sammed18/node-docker-app'
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
                    dockerImage = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                    echo "Docker image built: ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push()
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
            cleanWs()
        }
    }
}