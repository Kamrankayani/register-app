pipeline {
    agent {
        label 'jenkins-agent'
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven 'maven3'
        jdk 'java17'
    }

    stages {
        stage('clean workspace'){
            steps {
                cleanWs()
            }
        }
        stage('git repo') {
            steps {
                // Get some code from a GitHub repository
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Kamrankayani/register-app.git'
               
            }
        }
        stage('build'){
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('test'){
            steps {
                sh 'mvn test'
            }
        }
        stage('sonaqube analysis'){
            steps {
                script {
                withSonarQubeEnv(credentialsId: 'sonar-token') {
                 sh 'mvn sonar:sonar'
                        }
                }
            }
        }
        
        stage('Quality gate'){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('docker build and push'){
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                   def docker_image = docker.build ('kamrandevops23/register-app-pipeline')
                   docker_image.push("${env.BUILD_NUMBER}")
                   docker_image.push("latest")

                    }
                }
            }
        }
            
        
    }
}

