pipeline {
    agent any

    environment {
        DOCKERHUB_CREDS = 'dockerhub-creds' // credenziali in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDS,
                                                  usernameVariable: 'DOCKER_USER',
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh """
                docker compose pull
                docker compose up -d
                """
            }
        }
    }
}

