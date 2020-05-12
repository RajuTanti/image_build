node {    
    def newApp
    def registry = 'tantiraju/microservices-nod'
    def registryCredential = 'dockerhub'
	
	stage('Git') {
		git 'https://github.com/RajuTanti/image_build.git'
	}
	stage('Build') {
		sh 'npm install'
	}
	stage('Test') {
		sh 'npm test'
	}
	stage('Building image') {
        docker.withRegistry(registryCredential ) {
		    def buildName = registry + ":$BUILD_NUMBER"
			newApp = docker.build buildName
			newApp.push()
        }
	}
	stage('Deploy Image') {
           steps{
		 script {
      			docker.withRegistry( '', registryCredential ) {
        		dockerImage.push()
      			}
	      }
  	   }
	}	
       stage('Removing image') {
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker rmi $registry:latest"
    }
    
}
