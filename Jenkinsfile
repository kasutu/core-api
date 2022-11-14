
pipeline {
  agent any

  tools {
    maven 'Maven'
  }

  stages {
    stage('build') {
      steps {
        echo 'Checking out...'

        checkout([$class: 'GitSCM', branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[credentialsId: '0e3807c4-6832-48f3-ba4c-d360c5ba58f9', url: 'https://github.com/Kasutu/core-api']]])
        sh 'sudo mvn clean install'
      }
    }

    stage('test') {
      steps {
        echo 'teting app...'

        sh 'sudo mvn test'
      }
    }

    stage('install') {
      steps {
        echo 'compiling app...'

        sh 'sudo mvn install'
      }
    }

    stage('build docker image') {
      steps {
        script{
          sh 'sudo docker build -t kasutu/coreapi'
        }
      }
    }

    stage('deploy') {
      steps {
        echo 'pushing to docker hub...'

        script {
          withCredentials([string(credentialsId: 'docker-credentials-kasutu', variable: 'docker-credentials')]) {
            sh "sudo docker login -u kasutu -p ${docker-credentials}"
          }

          sh "sudo docker push kasutu/coreapi:latest"
        }
      }
    }
  }
}
