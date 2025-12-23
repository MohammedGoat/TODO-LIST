pipeline {
    agent any

    environment {
        DOCKERHUB_CREDS = 'dockerhub'
        BACKEND_IMAGE = 'slimox891/backend:latest'
        FRONTEND_IMAGE = 'slimox891/frontend:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -f Dockerfile.backend -t $BACKEND_IMAGE .'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -f Dockerfile.frontend -t $FRONTEND_IMAGE .'
            }
        }

        stage('Push Images') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
              echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
              docker push slimox891/backend:latest
              docker push slimox891/frontend:latest
            '''
        }
    }
}


        stage('Deploy') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d'
            }
        }
    }
}
