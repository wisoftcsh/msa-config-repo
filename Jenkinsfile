pipeline {
  agent any
  tools {
    jdk 'jdk-8u202'
  }
  stages {
    stage('Test & Build') {
      steps {
        dir('config-server') {
          sh './gradlew test'
          sh './gradlew bootJar'
        }
        dir('eureka') {
          sh './gradlew test'
          sh './gradlew bootJar'
        }
        dir('gateway') {
          sh './gradlew test'
          sh './gradlew bootJar'
        }
        dir('product') {
          sh './gradlew test'
          sh './gradlew bootJar'
        }
        dir('review') {
          sh './gradlew test'
          sh './gradlew bootJar'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build('config-server')
          docker.build('eureka')
          docker.build('gateway')
          docker.build('product')
          docker.build('review')
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        sh "docker-compose up -d"
      }
    }
  }
}
