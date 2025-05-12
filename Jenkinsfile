pipeline {
    agent any

    environment {
        IMAGE_NAME = 'newindia90/cafe-app'
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/Ankur-project/Cafe-App.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:latest .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh 'docker push $IMAGE_NAME:latest'
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/cafe-app-deployment.yml'
                    sh 'kubectl apply -f k8s/cafe-app-service.yml'
                }
            }
        }
    }
}
