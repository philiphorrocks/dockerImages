pipeline {

  environment {

    registry = "eu.gcr.io/terraform-243812"
    registryCredential = "gcr:terraform-243812"
    dockerImage = ''
    app = "testapp"
    git_tag = sh(returnStdout: true, script: 'git tags -l | head -1').trim()

  }
  
  agent any
  
  stages {
  
    stage('Cloning Git') {
      steps 
        git 'https://github.com/philiphorrocks/dockerImages.git'
      }
    }


  stage('Building image') {
      steps{
        script {
          
          dockerImage = docker.build "$registry/$app:$git_tag"
        }
      }
    }  


  stage('Deploy docker image to registry') {
      steps{
        script {
          
          docker.withRegistry('https://eu.gcr.io', registryCredential ) {
            dockerImage.push("$git_tag")
            dockerImage.push("latest")
          }
        }
      }
    }

  stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry/$app:$git_tag"
      }
    }
  }