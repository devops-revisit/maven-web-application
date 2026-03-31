pipeline
{
	agent any
	tools {
		maven "maven3.9"
	}
	environment {
		IMAGE_NAME = "maven-web-app"
		CONTAINER_NAME = "maven-web-app"
		PORT = "8082"
	}
	stages {
		stage('CheckOut') {
			steps {
				git credentialsId: 'github', url: 'https://github.com/devops-revisit/maven-web-application.git'
			}

		}
		stage('Build') {
			steps {
				sh 'mvn clean package -DiskipTest'
			}
		}
		stage('Verify Build') {
			steps {
				sh 'ls -l target/'
			}
		}
		stage('Docker Build Image') {
			steps {
				sh 'docker build -t mnidevops/$IMAGE_NAME:$BUILD_NUMBER .'
			}
		}
		stage('Docker Push Image') {
			steps {
				withCredentials([usernamePassword(
	            		credentialsId: 'docker-creds',
        	    		usernameVariable: 'DOCKER_USER',
            			passwordVariable: 'DOCKER_PASS'
        				)]) {
						sh '''
						echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin
         		   			docker push mnidevops/maven-web-app:${BUILD_NUMBER}
            					'''
        				}
			}
		}
		stage('Pull Image') {
			steps {
				sh 'docker pull mnidevops/maven-web-app:${BUILD_NUMBER}'
			}
		}
		stage('Stop Old Container') {
			steps {
				sh '''
				docker stop ${CONTAINER_NAME} || true
				docker rm ${CONTAINER_NAME} || true
				'''
			}
		}
	}
}
