pipeline {
  agent any

  environment {
    DOCKER_HUB_USER = "karthickdevops"
    IMAGE_NAME = "nodejs-k8s-app"
    IMAGE_TAG = "v${BUILD_NUMBER}" // 👈 Create version like v1, v2, v3
    FULL_IMAGE_NAME = "${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
  }

  stages {
    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${FULL_IMAGE_NAME}", "--no-cache .")
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
        script {
          sh '''
            sed -i 's|image: .*|image: '"$FULL_IMAGE_NAME"'|' k8s-deployment.yaml
            kubectl apply -f k8s-deployment.yaml --validate=false
            kubectl rollout status deployment/nodejs-app
          '''
        }
      }
    }
  }
}

