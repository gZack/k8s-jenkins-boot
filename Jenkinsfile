pipeline {
  environment {
	registry = "zekariyas/k8s-jenkins-boot"
	registryCredential = 'dockerhub_id'
  }
  agent {
    kubernetes {
      label 'jenkins-jenkins-agent'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 1  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project
      defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage('Build') {
      steps {  // no container directive is needed as the maven container is the default
        sh "mvn clean package"
      }
    }
    stage('Push') {
	  steps {
		container('docker') {
		  //sh "docker build -t k8s-jenkins-boot:$BUILD_NUMBER ."
		  //sh "docker push k8s-jenkins-boot:$BUILD_NUMBER"
		  script {
		  dockerImage = docker.build registry + ":$BUILD_NUMBER"
			  docker.withRegistry( '', registryCredential ) {
				  dockerImage.push()
			  }
		  }
		}
	  }
	}
  }
}