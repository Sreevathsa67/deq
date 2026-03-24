pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sreevathsa221/app.py"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Fixed the extra space in the URL
                git branch: 'main', url: 'https://github.com/Sreevathsa67/deq.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // This requires the 'Docker Pipeline' plugin to be installed in Jenkins
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Login and Push') {
            steps {
                script {
                    // Combining login and push into one clean block
                    // docker.withRegistry handles authentication automatically
                    docker.withRegistry('', 'dockerhub-creds') {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
