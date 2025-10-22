pipeline {
    agent any
    environment {
        DOCKERHUB = 'amthul/bookreview-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentialstoken', url: 'https://github.com/amthul/book-review-ci-cd.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB}:latest")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        docker.image("${DOCKERHUB}:latest").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }
    triggers {
        pollSCM('* * * * *')
    }
}
