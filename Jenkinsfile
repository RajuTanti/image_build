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
	//reading data from json
	
    	stage("checkout") {
	def jsonString = '{"name":"katone","age":5}'
	def jsonObj = readJSON text: jsonString

	assert jsonObj['name'] == 'katone'  // this is a comparison.  It returns true
	sh "echo ${jsonObj.name}"  // prints out katone
	sh "echo ${jsonObj.age}"   // prints out 5
    	}

	
	//end json
	stage('Building image') {
        docker.withRegistry('' , registryCredential ) {
		    def buildName = registry + ":$BUILD_NUMBER"
			newImage = docker.build buildName
		 	newImage.push()
        }
	}
	stage('Registring image') {
        docker.withRegistry( '' , registryCredential ) {
    		newImage.push 'latest'
        }
	}
    stage('Removing image') {
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker rmi $registry:latest"
    }
    
}
