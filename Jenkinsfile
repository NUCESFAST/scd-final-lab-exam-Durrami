pipeline {
    agent any

    environment {
        registry = "durrami/scd_final"
        DOCKER_CREDENTIALS = '8c231227-9fb5-4f4c-bb9c-20bae4c863fb'
        dockerImage = ''
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    dockerImage = docker.build(registry + ":$BUILD_NUMBER", "./Classrooms") // Specify the Dockerfile location
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub
                    docker.withRegistry('', DOCKER_CREDENTIALS) {
                        // Push the built image to Docker Hub
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy using Docker Compose
                    sh 'docker-compose -f ./Classrooms/docker-compose.yml up -d' // Specify the path to docker-compose.yml
                }
            }
        }
    }

    post {
        always {
            // Cleanup
            script {
                // Stop and remove containers after use
                sh 'docker-compose -f ./Classrooms/docker-compose.yml down' // Specify the path to docker-compose.yml
            }
        }
    }
}
