pipeline{
	agent any
	environment {
      	    registry = "pylagg/first_repo"
	    registryCredential = 'docker_hub'
    	}
	  stages {
	      stage('Deployment')
		{
      			agent{ label 'docker_slave' }
			steps{
				  sh 'kubectl apply -f hextris.yml'
				  sh 'kubectl get all'
			}
		}
	      stage('Enter input'){  
		steps {
			input('Do you want to proceed?')
        	}
	     }
              stage('Deploying new version')
		{
      			agent{ label 'docker_slave' }
			steps{
				  sh 'kubectl set image deployment/hextris-dep hextris=pylagg/first_repo:version2'
				  sh 'kubectl rollout status deployment hextris-dep'
			}
		}
	  }
}
