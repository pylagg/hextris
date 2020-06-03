pipeline{
	agent {
		label 'docker_slave'
	}
	environment {
      	    registry = "pylagg/first_repo"
	    registryCredential = 'docker_hub'
    	}
	  stages {
		  stage('Configuring cluster') {
			  steps{
				  sh 'aws eks --region us-west-2 update-kubeconfig --name hextris_cluster'
				  sh 'kubectl get svc'
			  }
		  }
		stage('Docker Image Build for version_1.01') {
			 steps{
				sh 'docker build -t pylagg/first_repo:version_1.01 .'
				}
  		}
		  
		stage('Version_1.01 image push to docker hub') {
			 steps{
				 sh 'docker push pylagg/first_repo:version_1.01'
				}
  		}
		  	stage('Run docker image version_1.01'){
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
				sh 'docker run -d --name con1 -p 8080:80 pylagg/first_repo:version_1.01'
			}
		}
			
	      stage('Deploying version_1.01')
		{
      			steps{
				  sh 'kubectl apply -f hextris.yml'
				  sh 'kubectl get all'
				  sh 'kubectl get svc service-hextris -o yaml'
			}
		}	
		  	
	  }
}
