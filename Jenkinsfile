pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
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

