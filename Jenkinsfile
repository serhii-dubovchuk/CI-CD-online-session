pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:latest")
        }

      }
    }

    stage('unit-test') {
      steps {
        script {
          docker.image("${registry}:latest").inside{ c->
            sh 'python app_test.py'
          }
        }
      }
    }

    stage('Publish') {
      steps {
        script {
          docker.withRegistry("", "dockerhub-id"){
            docker.image("${registry}:latest").push("latest")
          }
        }

      }
    }

  }
  environment {
    registry = 'serhiidubovchuk/flask-app'
  }
}
