pipeline {
  environment {
    imagename = "nidhish07/jenkins-docker"
    registryCredential = 'nidhish-dockerhub'
    dockerImage = ''
    repoUrl = 'https://github.com/Nidhish-07/Docker-Jenkins'
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/Nidhish-07/Docker-Jenkins', branch: 'main'])
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Running image') {
      steps{
        script {
          sh "docker run ${imagename}:latest"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
            sh """
            git tag -a ${BUILD_NUMBER} -m "build number ${BUILD_NUMBER}"
            git push ${repoUrl} ${BUILD_NUMBER}
            """
          }
        }
      }
    }
  }
}
