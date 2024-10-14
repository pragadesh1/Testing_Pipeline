pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "pragadesh007/your_app_image"
  }

  stages {
    stage('Clone Repository') {
      steps {
        git branch: 'main', url: 'https://github.com/pragadesh1/Testing_Pipeline'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${DOCKER_IMAGE}:latest")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Deploy Using Ansible') {
      steps {
        ansiblePlaybook(
          playbook: 'ansible/deploy.yml',
          inventory: 'ansible/hosts.ini'
        )
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}
