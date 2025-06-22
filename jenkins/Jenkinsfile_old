pipeline {
  agent any

  environment {
    GITKRAFT_BIN = './gitkraft' // путь к CLI GitKraft
  }

  stages {
    stage('Build & Push Docker') {
      steps {
        sh 'docker build -t your-dockerhub-username/backend ./backend'
        sh "${GITKRAFT_BIN} record --stage=build --component=backend"

        sh 'docker build -t your-dockerhub-username/frontend ./frontend'
        sh "${GITKRAFT_BIN} record --stage=build --component=frontend"

        sh 'docker push your-dockerhub-username/backend'
        sh "${GITKRAFT_BIN} record --stage=push --component=backend"

        sh 'docker push your-dockerhub-username/frontend'
        sh "${GITKRAFT_BIN} record --stage=push --component=frontend"
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/backend-deployment.yml'
        sh "${GITKRAFT_BIN} record --stage=deploy --component=backend"

        sh 'kubectl apply -f k8s/backend-service.yml'
        sh 'kubectl apply -f k8s/frontend-deployment.yml'
        sh 'kubectl apply -f k8s/frontend-service.yml'
        sh "${GITKRAFT_BIN} record --stage=deploy --component=frontend"
      }
    }
  }

  post {
    always {
      sh "${GITKRAFT_BIN} push"
    }
  }
}
