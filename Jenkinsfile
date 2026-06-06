pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'eval $(minikube docker-env) && docker build -t flask-cicd .'
            }
        }

        stage('Deploy To Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
