pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'blamp2'
        DOCKER_IMAGE = 'blamp2/final'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/Blamp25/lampkibs-prototype.git']]])
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Run Docker commands inside a Docker container
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image('blamp2/final:latest').inside {
                            // Build Docker image
                            sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
                        }
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push Docker Image to DockerHub
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}


