pipeline {
    agent any
    environment {
        PROJECT_ID = 'swe645hw2'
        CLUSTER_NAME = 'swe645hw2'
        LOCATION = 'us-east-1a'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('BuildWAR') {
            steps {
            
            	dir('SWE645HW1PT2') {
            		echo 'Creating the Jar ...'
					sh 'java -version'
					sh 'jar -cvf Hw1.war *'
            	}
            }
        }
        
        stage("Build image") {
            steps {
                script {
                    ourapp = docker.build("sainitin43/stu_survey:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                	sh 'docker login -u sainitin43 -p SAInitin@123'
					ourapp.push("${env.BUILD_ID}")
                }
            }
        }        
        stage("UpdateDeployment") {
			steps{
				sh 'kubectl config view'
				sh "kubectl get deployments"
				sh "kubectl set image deployment/hw2dep container-0= sainitin43/SWE-645:${env.BUILD_ID}"
			}
		}
    }    
}