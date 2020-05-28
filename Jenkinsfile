pipeline{
	agent {
		label 'docker_slave'
	}
	environment {
      	    registry = "pylagg/first_repo"
	          registryCredential = 'docker_hub'
    	}
	  stages {
		stage('Docker Image Build for version2') {
			 steps{
				sh 'docker build -t pylagg/first_repo:version2 .'
				}
  		}
		  
		stage('Version2 image push to docker hub') {
			 steps{
				 sh 'docker push pylagg/first_repo:version2'
				}
  		}
		  	stage('Run docker image version2'){
			agent {
			label 'docker_slave'
			}
			steps{
				sh( script: '''#!/bin/bash
           				 if [ "$(docker ps -aq -f name=con2)" ]; then
					docker stop con2
					docker rm con2
					fi
        				'''.stripIndent())
				sh 'docker run -d --name con2 -p 8081:80 pylagg/first_repo:version2'
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
