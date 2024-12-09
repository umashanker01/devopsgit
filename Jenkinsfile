pipeline {
	agent {
	    label 'node1'
	}
	 tools {
        // Using the JDK and Maven tools configured in Global Tool Configuration
        //jdk 'JDK 11'  // Name of the JDK configuration
        maven 'mvn'  // Name of the Maven configuration
        }
 	environment {
		// DOCKER_TAG = getDockerTag()
		ENV = 'develop'
		DOCKERHUB_CREDENTIALS_ID = "7f9ba4ff-b64d-4cc4-8518-ee8d0d60ae73"
        }
	stages {
		stage('checkout') {
			steps {
		             // start with  clean workspce
			     deleteDir()
		             //checkout branch
		             checkout scm
			}
		}
		stage('build') {
			steps {
				sh "mvn clean package -DskipTests -Djacoco.skip=true"
		                sh "docker build . -t  skr1819/skrknowledge/${ENV}"
			}
		}
		stage('Docker build image && push') {
			steps {
			    script {
			       docker.withRegistry('https://registry.hub.docker.com', "${DOCKERHUB_CREDENTIALS_ID}") {
                       sh "docker push skr1819/skrknowledge/${ENV}:latest"
                   }
			    }
			}
		}
		stage('deploy to kuberneters') {
			steps {
				sh "docker images"
			}
		}
    }
}
