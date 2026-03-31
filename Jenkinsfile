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
	}
}
