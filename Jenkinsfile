pipeline {
agent any

```
environment {
    KUBECONFIG = "/var/lib/jenkins/.kube/config"
}

stages {

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t flask-cicd .'
        }
    }

    stage('Deploy To Kubernetes') {
        steps {
            sh 'kubectl apply -f deployment.yaml'
            sh 'kubectl apply -f service.yaml'
        }
    }
}
```

}

