pipeline {
  agent any
  options {
    skipDefaultCheckout()   // prevents Declarative: Checkout SCM
  }
  environment {
    IMAGE = "xzenintojix/mstip-flask"
    SHORT_SHA = "${env.BUILD_NUMBER}"
  }
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE:$SHORT_SHA -t $IMAGE:latest .'
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
          sh 'echo $DH_PASS | docker login -u $DH_USER --password-stdin'
          sh 'docker push $IMAGE:$SHORT_SHA'
          sh 'docker push $IMAGE:latest'
        }
      }
    }
  }
}
