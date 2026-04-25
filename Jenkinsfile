pipeline {
	options { 
		timeout(time: 5, unit: 'MINUTES') 
	}
	agent {
		label 'master'
	}
	parameters{
		string(name: 'REPO_URL', description: 'JAVA APPLICATIION GIT REPO URL')
		string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'BRANCH NAME')
	}
	environment {
		NAME = "Litesh Zadane"
	}
	tools{
		maven 'mavenV3.9.15'
	}
	stages{
		stage("Cleaning Workspace"){
			steps {
				sh 'rm -Irvf *'
			}
		}
		stage("Cloning Git Repo stage"){
			steps{
				sh "git clone -b ${params.BRANCH_NAME} ${params.REPO_URL}"
				sh 'echo "Job is runned by $NAME"'
			}
		}
		stage("Detect Repo Name") {
			steps {
				script {
					env.REPO_NAME = params.REPO_URL.tokenize("/").last().replace(".git","")
					echo "REPO_NAME: ${env.REPO_NAME}"
				}
			}
		}
		stage("Maven Build Package Stage"){
			steps {
				sh 'echo "mvn package is building..."'
				dir(REPO_NAME){
					sh 'mvn clean package -X'
				}
			}
		}
		stage("Deploy .war file to Tomcat9 server"){
			steps{
				dir(REPO_NAME){
					sh 'echo "Deploying .war file to tomcat server"'
					sh 'sudo cp -rvf ./target/*.war /usr/share/tomcat/webapps/apps.war'
				}
			}
		}
	}
}
