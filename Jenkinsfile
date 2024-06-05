pipeline {
    agent any

    environment {
        registry = "durrami/scd_final"
        DOCKER_CREDENTIALS = '8c231227-9fb5-4f4c-bb9c-20bae4c863fb'
        dockerImage = ''
    }

    stages {
        stage('Checkout') {
    steps {
        git branch: 'main', url: 'https://github.com/NUCESFAST/scd-final-lab-exam-Durrami.git'
    }
}

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub and push the built image
                    docker.withRegistry('', DOCKER_CREDENTIALS) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:80 21I_1164_Backend:latest'
            }
        }

        stage('Validate Application') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
    }

    post {
        always {
            // Cleanup
            script {
                // Stop and remove containers after use
                sh 'docker-compose down'
            }
        }
    }
}
