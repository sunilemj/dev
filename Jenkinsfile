pipeline {
  agent any
  environment {
    IMAGE = "sunilemj/myweb"
    TAG   = "v${env.BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE:$TAG .'
      }
    }
    stage('Login to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
          sh 'echo $DH_PASS | docker login -u $DH_USER --password-stdin'
        }
      }
    }
    stage('Push Image') {
      steps {
        sh 'docker push $IMAGE:$TAG'
        sh 'docker tag $IMAGE:$TAG $IMAGE:latest'
        sh 'docker push $IMAGE:latest'
      }
    }
  }
  post {
    always { sh 'docker logout || true' }
  }
}
