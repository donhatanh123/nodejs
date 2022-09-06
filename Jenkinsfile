pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKER_CREDENTIALS = credentials('nodejsapp')
    apinodejsTag1=sh(returnStdout: true, script: "git tag --sort version:refname | tail -1").trim()
  }
  stages {
    stage('Build') {
      steps {
        sh "printenv"
         script {
           env.apinodejsTag=$apinodejsTag1
        }
        echo " apinodejsTag = ${env.apinodejsTag} origin var =$apinodejsTag1"
        sh 'docker build -t registry.gitlab.com/xzhoang/nodejsmysql:$apinodejsTag  . '
      }
    }
    stage('Login') {
      steps {
        
	echo "Docker Login"
        sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR registry.gitlab.com/xzhoang/nodejsmysql   --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push registry.gitlab.com/xzhoang/nodejsmysql:$apinodejsTag '
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

