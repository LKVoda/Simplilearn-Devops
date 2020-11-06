pipeline {
  environment {
    registry = "Lee Keene/Docker-Jenkins"
    registryCredential = 'lkvodadockerhub'
  }
  agent any
  stages {
	stage('Cloning Git') {
	steps {
		git 'https://github.com/lkvoda/simplilearn-devops.git'
	      }
	}

        stage('Building image') {
        steps{
            script {
            dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
        }
        }

        stage('Deploy Image') {
        steps{
            script {
            docker.withRegistry( '', 'dockerhub' ) {
                dockerImage.push()
            }
            }
        }
        }

        stage('Remove Image') {
        steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
        }
        }
   }   
}

node {
    stage('Execute Image'){
        def customImage = docker.build("Lee Keene/Docker-Jenkins:${env.BUILD_NUMBER}")
        customImage.inside {
            sh 'echo This is the code executing inside the container.'
        }
    }
}
