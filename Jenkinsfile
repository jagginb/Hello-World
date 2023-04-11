pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
            DOCKER_LOGIN_CREDENTIALS=credentials('docker-jenkins')
    }

stages {
  stage('Checkout') {
    steps {
        git 'https://github.com/jagginb/Hello-World.git'
    }
  }

  stage('Build') {
    steps {
        sh 'mvn clean install'
        sh 'docker build --tag=jagginb/docker:$BUILD_NUMBER .'
    }
  }

  stage('Docker login and Docker push') {
    steps {
        sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
        sh 'docker push jagginb/docker:$BUILD_NUMBER'
     
    }
  }

  stage('Docker deploy') {
    steps {
        sh "docker run -itd -p 82:8080 jagginb/docker:$BUILD_NUMBER"
      
    }
  }
 }
}