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
	
    	stage("readJson") {
	 

	def props = readJSON file: 'package1.json'
	assert props['attr1'] == 'One'
	assert props.attr1 == 'One'

	def props = readJSON text: '{ "key": "value" }'
	assert props['key'] == 'value'
	assert props.key == 'value'

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
