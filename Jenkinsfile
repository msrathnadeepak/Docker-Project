pipeline {
  environment {
    registry = "msrathnadeepak"
    dockerImage = ""
  }

  agent any

  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/msrathnadeepak/Docker-Project.git'
      }
    }

    stage('Build Python Flask Image') {
      steps {
        script {
          // Build the Python Flask image
          dockerImage = docker.build registry + "/python-flask:$BUILD_NUMBER", "-f flask/Dockerfile ."
        }
      }
    }

    stage('Push Python Flask Image') {
      steps {
        script {
          // Push the Python Flask image to the Docker registry
          docker.withRegistry([credentialsId: 'docker-hub-credentials', url: 'https://hub.docker.com/repositories/msrathnadeepak']) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Build MySQL Image') {
      steps {
        script {
          // Build the MySQL image
          docker.build registry + "/mysql:$BUILD_NUMBER", "-f mysql/Dockerfile ."
        }
      }
    }

    stage('Push MySQL Image') {
      steps {
        script {
          // Push the MySQL image to the Docker registry
          docker.withRegistry([credentialsId: 'docker-hub-credentials', url: 'https://hub.docker.com/repositories/msrathnadeepak']) {
            docker.push registry + "/mysql:$BUILD_NUMBER"
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          // Deploy your application to Kubernetes (adjust this step according to your Kubernetes deployment method)
          sh 'kubectl apply -f frontend.yaml'
        }
      }
    }
  }
}
