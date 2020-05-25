pipeline{
	agent any
	environment {
      	    registry = "pylagg/first_repo"
	    registryCredential = 'docker_hub'
    	}
	  stages {
        	stage("Code Checkout") {
                  steps {
                	git branch: 'version1',
                	url: 'https://github.com/pylagg/hextris.git'
                  }
              }
	      stage('Deployment')
		{
      			agent{ label 'docker_slave' }
			steps{
				  sh 'kubectl apply -f hextris1.yml'
				  sh 'kubect get all'
			}
		}
	  }
	
	post {
		 
   		 failure {
      			  mail to: 'pk1932ag@gmail.com',
             		subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             		body: "Something is wrong with ${env.BUILD_URL}"
    		}
		success{
      			  mail to: 'pk1932ag@gmail.com',
             		subject: "Success Pipeline: ${currentBuild.fullDisplayName}",
             		body: "Congratulations ${env.BUILD_URL}"
    		}
		}
	
}
