pipeline {
    agent { label 'ansiblesrv' }
    tools {
        jdk 'java17'
        maven 'maven'
    }
   
    //	environment {
	//    APP_NAME = "register-app-pipeline"
    //        RELEASE = "1.0.0"
    //        DOCKER_USER = "mukeshjadav7696"
    //        DOCKER_PASS = 'dockerhub'
    //        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    //        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	//    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
  
   //}
   
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github-mukeshjadav2025', url: 'https://github.com/mukeshjadav2025/register-app'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
		
       }

       	stage("Test Application"){
           	steps {
                 sh "mvn test"
           	}
       	}
		stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'jenkins_sonarqube_token') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
        }
	    
       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins_sonarqube_token'
               }	
           }
       }		
	}
}
