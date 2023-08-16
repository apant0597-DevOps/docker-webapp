currentBuild.displayName = "Maven-App-"+currentBuild.number
pipeline {
    agent any
    tools {
	    maven 'maven-jenkins'
    }
    stages {
        stage('MAVEN BUILD'){
            steps {
                sh 'mvn clean package'
		        sh 'mv target/*.war target/tomcat-app.war'
            }
        }
	    stage('DOCKER BUILD'){
            steps {
                sh '''
				
				docker build . -t maven-webapp:v1
				
				'''
            }  
        }
	    stage('DOCKER PUSH'){
            steps {
                withCredentials([string(credentialsId: 'docker-cred', variable: 'dockerhubcred')]) {
                sh '''
				
				docker login -u apant0597 -p ${dockerhubcred}
				docker push apant0597/maven-webapp:v1
				
				'''
            }
                
            }  
        }
    }
    	post {
            success { 
            	echo "This pipeline is successfull!"
            }
    	    unsuccessful {
            	echo "ISSUE!!!"
            }
    	}
}
