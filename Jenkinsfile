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
				sh 'docker build -t pylagg/first_repo:version1 .'
				}
  		}
		  
		stage('Version1 image push to docker hub') {
			 steps{
				 sh 'docker push pylagg/first_repo:version1'
				}
  		}
		  	stage('Run docker image version1'){
			agent {
			label 'docker_slave'
			}
			steps{
				sh( script: '''#!/bin/bash
           				 if [ "$(docker ps -aq -f name=con1)" ]; then
					docker stop con1
					docker rm con1
					fi
        				'''.stripIndent())
				sh 'docker run -d --name con1 -p 8080:80 pylagg/first_repo:version1'
			}
		}
			
	      stage('Deploying version1')
		{
      			steps{
				  sh 'kubectl apply -f hextris.yml'
				  sh 'kubectl get all'
				  sh 'kubectl get svc service-hextris -o yaml'
				  sh 'kubectl rollout status deployment hextris-dep'
			}
		}	
		  	
	  }
}
