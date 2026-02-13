pipeline {
    agent any

    environment {
        DOCKERHUB_CREDS = 'dockerhub-creds' // credenziali Docker Hub in Jenkins
        FRONTEND_IMAGE = 'jacktheskunk/app-frontend:latest'
        BACKEND_IMAGE  = 'jacktheskunk/app-backend:latest'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: env.DOCKERHUB_CREDS,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh """
                docker build -t $FRONTEND_IMAGE 
                docker build -t $BACKEND_IMAGE 
                """
            }
        }

        stage('Push Docker Images') {
            steps {
                sh """
                docker push $FRONTEND_IMAGE
                docker push $BACKEND_IMAGE
                """
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                // usa build images locali invece di pull
                sh """
                docker compose up -d --build
                """
            }
        }
    }
}

