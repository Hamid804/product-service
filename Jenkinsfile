pipeline {

	agent any
	
	options {
		timestamps()
		// skipDefaultCheckout()
		timeout(time: 60, unit: 'MINUTES')
		buildDiscarder(logRotator(numToKeepStr:'10'))
	}
    
	stages{
	
		stage('Hello') {
			steps {
				echo "Hello Hamid"
			}
		}
		
		stage('Build') {
			steps {
				bat 'mvn clean install'
			}
		}
		
		stage('Test') {
			steps {
				bat 'mvn test'
			}
			post {
				always {
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		
		stage('Build Docker') {
			when {
				branch "master"
			}
			steps {
				script {
					bat 'docker build -t backend/product-service .'
				}
			}
		}
		
		stage('Push to DockerHub') {
			when {
				branch "master"
			}
			steps {
				script {
					withCredentials([string(credentialsId: 'dockerhub-password', variable: 'dockerhubpwd')]) {
						bat 'docker login -u hamid804 -p ${dockerhubpwd}'
					}
					
					bat 'docker push backend/product-service'
				}
			}
		}
	}
}

