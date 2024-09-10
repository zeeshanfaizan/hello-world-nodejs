pipeline {
    agent any

    environment {
        // Define environment variables if needed
        DOCKER_IMAGE = 'my-nodejs-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from SCM
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run('-d -p 8080:80 --name my-nodejs-app')
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    // Wait for the application to start (optional)
                    sleep time: 30, unit: 'SECONDS'

                    // Run tests against the running application
                    // You can use tools like curl, wget, or any testing framework
                    sh 'curl -f http://localhost:8080/ || exit 1'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Stop and remove the Docker container
                    sh 'docker stop my-nodejs-app || true'
                    sh 'docker rm my-nodejs-app || true'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images (optional)
            script {
                sh 'docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true'
            }
        }
    }
}
