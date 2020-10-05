pipeline {
environment {
	registry = "lkvoda/simplilearn-devops"
    registryCredential = "dockerhub"
}
agent any
stage {
	stage('Building image') {
    steps{
    	script {
        dockerImage = docker.build registry + ":$Build_number"
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
    	sh "dockerrmi $registry.$build_number"
    }
    }
}
}

node {
	stage('Execute Image') {
    	def customeImage = docker.build("lkvoda/simplilearn-devops:${env.build_number}")
        customImage.inside {
        	sh 'echo This is the code executing inside the container.'
        }
    }
}
