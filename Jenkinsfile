pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_qa', defaultValue: '172.31.9.194', description: 'Staging Server')
	         string(name: 'tomcat_prod', defaultValue: '172.31.5.36', description: 'Production Server')
	    }
	
	    triggers {
	         pollSCM('*/5 * * * *')
	     }
	
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
	
	        stage ('Deployments'){
	            parallel{
	                stage ('Deploy to QA'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Pipeline_Project/target/vprofile-v1.war devops@${params.tomcat_qa}:/usr/local/apache-tomcat-8.5.75/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Pipeline_Project/target/vprofile-v1.war devops@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.75/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
