pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('nodejsapp')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t registry.gitlab.com/xzhoang/nodejsmysql:latest  . '
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR registry.gitlab.com/xzhoang/nodejsmysql   --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push registry.gitlab.com/xzhoang/nodejsmysql:latest '
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

