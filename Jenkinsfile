pipeline{
	agent any
	environment {
      	    registry = "pylagg/first_repo"
	    registryCredential = 'docker_hub'
    	}
	  stages {
		stage('Docker Image Build for version1') {
 			 agent {
			 label 'docker_slave'
			 }
			 steps{
				sh 'docker build -t pylagg/first_repo:version1 .'
			}
  		}
		stage('Push version1 image to docker hub') {
			agent {
			label 'docker_slave'
			}
			steps{
				sh 'docker push pylagg/first_repo:version1'
			}
		}		  
	      stage('Deploying version1')
		{
      			agent{ label 'docker_slave' }
			steps{
				  sh 'kubectl apply -f hextris.yml'
				  sh 'kubectl get all'
			}
		}
		stage('Docker Image Build for version2') {
 			 agent {
			 label 'docker_slave'
			 }
			 steps{
				 git branch: 'version2',
                		url : 'https://github.com/pylagg/hextris.git'
				sh 'docker build -t pylagg/first_repo:version2 .'
			}
  		}
		stage('Push version2 image to docker hub') {
			agent {
			label 'docker_slave'
			}
			steps{
				sh 'docker push pylagg/first_repo:version2'
			}
		}		  
	      stage('Enter input'){  
		steps {
			input('Do you want to proceed?')
        	}
	     }
              stage('Deploying version2')
		{
      			agent{ label 'docker_slave' }
			steps{
				  sh 'kubectl set image deployment/hextris-dep hextris=pylagg/first_repo:version2'
				  sh 'kubectl rollout status deployment hextris-dep'
			}
		}
	  }
}
