pipeline {
  environment {
    imagename = "vanvoo/pacman"
    registryCredential = 'vanvoo'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/vanvoo/pacman.git', branch: 'master', credentialsId: 'vanvoo-github-user-token'])
      }
    }
    stage('Building Image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploying Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }
    stage('Clean-up Docker Image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"
      }
    }
  }
}
