pipeline {
  agent any

  stages {
    stage ('Build Docker Image') {
      steps {
        script {
          dockerapp = docker.build("devvicente/kube-news:v${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
        }
      }
    }

    stage ('Push Docker Image') {
      steps {
        script {
          docker.withRegistry("https://registry.hub.docker.com", 'DOCKER_HUB') {
            dockerapp.push("v${env.BUILD_ID}")
            dockerapp.push('latest')
          }
        }
      }
    }

    stage ('Deploy Kubernetes') {
      environment {
        tag_version = "v${env.BUILD_ID}"
      }
      steps {
        script {
          withKubeConfig([credentialsId: 'KUBE_CONFIG']) {
            sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
            sh 'kubectl apply -f ./k8s/deployment.yaml'
          }
        }
      }
    }
  }
}