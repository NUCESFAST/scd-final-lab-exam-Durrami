pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git branch: 'main', credentialsId: 'your-github-credentials', url: 'https://github.com/yourusername/yourrepository.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies (if any)
                sh 'npm install'  // Example command for Node.js project
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                sh 'docker build -t yourimage:latest .'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                // Login to Docker Hub
                withCredentials([usernamePassword(credentialsId: 'your-dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                }

                // Push Docker image to Docker Hub
                sh 'docker push yourimage:latest'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Run Docker container
                sh 'docker run -d -p 8080:80 yourimage:latest'  // Example command, adjust ports as needed
            }
        }

        stage('Validate Application') {
            steps {
                // Execute tests or perform validation
                // Example: sh 'npm test'
            }
        }
    }
}
