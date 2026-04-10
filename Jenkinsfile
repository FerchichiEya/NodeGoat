pipeline {
  agent any

  environment {
    SONAR_HOST_URL = 'http://192.168.152.134:9000'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/FerchichiEya/NodeGoat.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t nodegoat-app .'
      }
    }

    stage('SonarQube Scan') {
      steps {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_TOKEN')]) {
          sh '''
            sonar-scanner \
              -Dsonar.projectKey=NodeGoat \
              -Dsonar.host.url=$SONAR_HOST_URL \
              -Dsonar.login=$SONAR_TOKEN
          '''
        }
      }
    }

    stage('Trivy Scan') {
      steps {
        sh 'trivy image --severity HIGH,CRITICAL --exit-code 1 nodegoat-app'
      }
    }
  }
}