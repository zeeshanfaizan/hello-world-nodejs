pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/zeeshanfaizan/hello-world-nodejs.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-image')
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('my-image').run('-d -p 8080:80')
                }
            }
        }
        stage('Test Application') {
            steps {
                script {
                    sh 'curl http://localhost:8080'
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    docker.image('my-image').remove()
                }
            }
        }
    }
}
