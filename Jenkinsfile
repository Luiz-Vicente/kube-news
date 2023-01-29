pipeline {
  agent any

  stages {
    stage ('Build Docker Image') {
      steps {
        script {
          dockerapp = docker.build("devvicente/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
        }
      }
    }

    stage ('Push Docker Image') {
      steps {
        script {
          docker.withRegistry("https://registry.hub.docker.com", 'DOCKER_HUB') {
            dockerapp.push("${env.BUILD_ID}")
            dockerapp.push('latest')
          }
        }
      }
    }
  }
}