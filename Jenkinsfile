pipeline{
	agent {
		label 'docker_slave'
	}
	environment {
      	    registry = "pylagg/first_repo"
	          registryCredential = 'docker_hub'
    	}
	  stages {
		stage('Docker Image Build for version_1.02') {
			 steps{
				sh 'docker build -t pylagg/first_repo:version_1.02 .'
				}
  		}
		  
		stage('Version_1.02 image push to docker hub') {
			 steps{
				 sh 'docker push pylagg/first_repo:version_1.02'
				}
  		}
		  	stage('Run docker image version_1.02'){
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
				sh 'docker run -d --name con2 -p 8081:80 pylagg/first_repo:version_1.02'
			}
		}
			
	      stage('Deploying version2')
		{
      			steps{
           			  sh 'kubectl apply -f hextris.yml'
				  sh 'kubectl get all'
				  sh 'kubectl get svc service-hextris -o yaml''
			}
		}	
		  	
	  }
}
