pipeline {
  agent any
  environment {
    IMAGE = "xzenintojix/mstip-flask"
    SHORT_SHA = "${env.GIT_COMMIT?.take(7) ?: 'local'}"
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
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f deployment.yaml -n mstip || true'
        sh 'kubectl apply -f service.yaml -n mstip || true'
        sh 'kubectl rollout status deploy/mstip -n mstip'
      }
    }
  }
}

