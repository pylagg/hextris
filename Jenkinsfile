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
				  sh 'kubect get all'
			}
		}
	  }
}
