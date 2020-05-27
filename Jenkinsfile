pipeline{
	agent {
		label 'docker_slave'
	}
	environment {
      	    registry = "pylagg/first_repo"
	    registryCredential = 'docker_hub'
    	}
	  stages {
		stage('Docker Image Build for version1') {
			 steps{
				git branch 'version1' 
				sh 'docker build -t pylagg/first_repo:version1 .'
				 sh 'docker push pylagg/first_repo:version1'
			}
  		}
	      stage('Deploying version1')
		{
      			steps{
				  sh 'kubectl apply -f hextris.yml'
				  sh 'kubectl get all'
			}
		}	
		  stage('Docker Image Build for version2') {
 			  
			 steps{
				git branch 'version2'
				sh 'docker build -t pylagg/first_repo:version2 .'
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
      			steps{
				  sh 'kubectl set image deployment/hextris-dep hextris=pylagg/first_repo:version2'
				  sh 'kubectl rollout status deployment hextris-dep'
			}
		}
	  }
}
