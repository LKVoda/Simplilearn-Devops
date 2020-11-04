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
	stage('Execute Image') {
    	def customeImage = docker.build("lkvoda/simplilearn-devops:${env.BUILD_NUMBER}")
        customImage.inside {
        	sh 'echo This is the code executing inside the container.'
        }
    }
}
