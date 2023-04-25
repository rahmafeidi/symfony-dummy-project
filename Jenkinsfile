pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/rahmafeidi/symfony-dummy-project.git'
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t rahmafeidi/nginx  docker/nginx/'
        sh 'docker build -t rahmafeidi/mysql  docker/mysql/'
        sh 'docker build -t rahmafeidi/php-fpm  docker/php-fpm/'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push rahmafeidi/nginx'
        sh 'docker push rahmafeidi/mysql'
        sh 'docker push rahmafeidi/php-fpm'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
