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

	def props = readJSON file: 'package1.json'
	assert props['attr1'] == 'One'
	assert props.attr1 == 'One'

	def props = readJSON text: '{ "key": "value" }'
	assert props['key'] == 'value'
	assert props.key == 'value'

	def props = readJSON text: '[ "a", "b"]'
	assert props[0] == 'a'
	assert props[1] == 'b'

	def props = readJSON text: '{ "key": null, "a": "b" }', returnPojo: true
	assert props['key'] == null
	props.each { key, value ->
	    echo "Walked through key $key and value $value"
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
