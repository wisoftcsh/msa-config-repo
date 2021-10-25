pipeline {
   environment {
		BUILD_VERSION = sh(script: "echo `date +%Y%m%d%H%M%S`", returnStdout: true).trim()
		DOCKER_HUB_DIR = 'csh9196'
	}
  agent any
  tools {
    jdk 'jdk11'
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
          dir('config-server') {
            sh "docker build . -t ${DOCKER_HUB_DIR}/config-server:${env.BUILD_VERSION}"
            sh "docker push ${DOCKER_HUB_DIR}/config-server:${env.BUILD_VERSION}"
          }
          dir('eureka') {
            sh "docker build . -t ${DOCKER_HUB_DIR}/eureka:${env.BUILD_VERSION}"
            sh "docker push ${DOCKER_HUB_DIR}/eureka:${env.BUILD_VERSION}"
          }
          dir('gateway') {
            sh "docker build . -t ${DOCKER_HUB_DIR}/gateway:${env.BUILD_VERSION}"
            sh "docker push ${DOCKER_HUB_DIR}/gateway:${env.BUILD_VERSION}"
          }
          dir('product') {
            sh "docker build . -t ${DOCKER_HUB_DIR}/product:${env.BUILD_VERSION}"
            sh "docker push ${DOCKER_HUB_DIR}/product:${env.BUILD_VERSION}"
          }
          dir('review') {
            sh "docker build . -t ${DOCKER_HUB_DIR}/review:${env.BUILD_VERSION}"
            sh "docker push ${DOCKER_HUB_DIR}/review:${env.BUILD_VERSION}"
          }`
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
