pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-nodejs-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    sh 'docker stop my-nodejs-app || true'
                    sh 'docker rm my-nodejs-app || true'
                    // sh 'docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run('-d -p 8081:3000 --name my-nodejs-app')
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    sleep time: 30, unit: 'SECONDS'
                    sh 'curl -f http://localhost:8081/ || exit 1'
                }
            }
        }

        
    }

    post {
        always {
            script {
                sh 'docker system prune -af || true'
            }
        }
    }
}
