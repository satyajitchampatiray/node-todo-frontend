pipeline {
  environment {
    registry = "dipankar435/docker-repo"
    registryCredential = 'dockerhub'
    dockerImage = 'node-app'
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/dipankar2019/node-todo-frontend'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        sh 'docker build --tag=node-app .'
      }
    }
    stage('Push Image') {
      steps{
         script {
            docker.withRegistry( 'node-app', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
