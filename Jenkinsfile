pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/zeeshanfaizan/hello-world-nodejs.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('hello-world-nodejs')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('hello-world-nodejs').run('-p 3000:3000')
                }
            }
        }

        stage('Test Application') {
            steps {
                script {
                    // Check if the app is running by testing the response from localhost
                    sh 'curl http://localhost:3000'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh 'docker ps -a | grep hello-world-nodejs | awk \'{print $1}\' | xargs docker rm -f'
                }
            }
        }
    }
}
