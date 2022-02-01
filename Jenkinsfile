pipeline {
	    agent any
	    stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	        stage ('Deploy to Dev'){
	            steps {
	                build job: 'VProfile-Deploy-Dev job'
	            }
	        }
	
	        stage ('Deploy to UAT'){
	            steps{
	                timeout(time:5, unit:'DAYS'){
	                    input message:'Approve PRODUCTION Deployment?'
	                }
	
	                build job: 'VProfile-Deploy-UAT job'
	            }
	            post {
	                success {
	                    echo 'Code deployed to Production.'
	                }
	
	                failure {
	                    echo ' Deployment failed.'
	                }
	            }
	        }
	
	
	    }
	}
