pipeline {
  agent any

  environment {
    IMAGE_NAME = "karthickdevops/nodejs-k8s-app:latest"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git credentialsId: 'github-credentials', url: 'https://github.com/admin971/k8s-deployment.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}")
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
        sh 'kubectl apply -f k8s-deployment.yaml'
      }
    }
  }
}

