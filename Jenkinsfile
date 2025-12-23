pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub'
        BACKEND_IMAGE = 'docker login -u slimox891/backend:latest'
        FRONTEND_IMAGE = 'docker login -u slimox891/frontend:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MohammedGoat/TODO-LIST.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    docker.build("${env.BACKEND_IMAGE}", "-f Dockerfile.backend .")
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    docker.build("${env.FRONTEND_IMAGE}", "-f Dockerfile.frontend .")
                }
            }
        }

        stage('Push Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${env.DOCKERHUB_CREDENTIALS}") {
                        docker.image("${env.BACKEND_IMAGE}").push()
                        docker.image("${env.FRONTEND_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
            }
        }
    }
}
