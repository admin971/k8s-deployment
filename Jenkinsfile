pipeline {
  agent any

  environment {
    IMAGE_NAME = "karthickdevops/nodejs-k8s-app:latest"
  }

  stages {
    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}", "--no-cache")
        }
      }
    }

    stage('Push Docker Image to DockerHub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          kubectl apply -f k8s-deployment.yaml --validate=false
          kubectl rollout status deployment/nodejs-app
        '''
      }
    }
  }
}

