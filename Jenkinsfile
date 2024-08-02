pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
   
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Kamrankayani/pipeline'
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
			   withSonarQubeEnv('SonarQube') {
                        sh "mvn sonar:sonar -Dsonar.login=${SONARQUBE_TOKEN}"
                    }
		      //  withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                       // sh "mvn sonar:sonar"
		        //}
	           }	
           }
       }
   }
}
