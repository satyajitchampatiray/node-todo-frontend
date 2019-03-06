pipeline {
  environment {
    registry = "dipankar435/docker-repo"
    registryCredential = 'dockerhub'
    dockerImage = ''
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
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Run Container') {
      def containerId = sh(script: 'docker ps -aqf "name=node-app"', returnStdout: true)
      steps {
        //containerId = sh(script: 'docker ps -aqf "name=node-app"', returnStdout: true)
        sh 'docker stop ${containerId}'
        sh 'docker rm ${containerId}'
        sh 'docker run --name=node-app -d -p 3000:3000 $registry:$BUILD_NUMBER &'
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
