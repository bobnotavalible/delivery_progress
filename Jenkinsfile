pipeline {
  agent any
  stages {
    stage('Build & Push Docker') {
      steps {
        sh 'docker build -t your-dockerhub-username/backend ./backend'
        sh 'docker build -t your-dockerhub-username/frontend ./frontend'
        sh 'docker push your-dockerhub-username/backend'
        sh 'docker push your-dockerhub-username/frontend'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/backend-deployment.yml'
        sh 'kubectl apply -f k8s/backend-service.yml'
        sh 'kubectl apply -f k8s/frontend-deployment.yml'
        sh 'kubectl apply -f k8s/frontend-service.yml'
      }
    }
  }
}
