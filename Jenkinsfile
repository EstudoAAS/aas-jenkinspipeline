pipeline {
	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: '50.112.201.240', description: 'Staging Server')
		string(name: 'tomcat_prod', defaultValue: '34.216.201.187', description: 'Production Server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages{
		stage('Build'){
			steps{
				sh 'mvn clean package'
			}
			post{
				sucess {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage ('Deployments') {
			parallel{
				stage('Deploy to Staging'){
					steps {
						sh "scp -i /home/ubuntu/.ssh/hudeing.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
					}
				}
				stage('Deploy to Production'){
					steps {
						sh "scp -i /home/ubuntu/.ssh/hudeing.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
					}
				}
			}
		}
	}
}
