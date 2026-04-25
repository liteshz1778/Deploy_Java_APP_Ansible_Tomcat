pipeline{
    options { 
        timeout(time: 5, unit: 'MINUTES') 
        retry(2)
    }
    agent {
        label 'master'
    }
    environment {
        NAME = "Litesh Zadane"
        BRANCH_NAME = "master"
        REPO_URL = "https://github.com/liteshz1778/spring-boot-war-example.git"
    }
    tools{
	maven 'mavenV3.9.15'
    }
    stages{
        stage("Cleaning Workspace"){
            steps {
               sh 'rm -rvf *'
            }
        }
        stage("Cloning Git Repo stage"){
            steps{
                sh 'echo "Cloning Git Repo..."'
		sh 'git clone -b ${BRANCH_NAME} ${REPO_URL}'
                sh 'echo "Job is runned by $NAME"'
            }
        }
		stage("Detect Repo Name") {
            steps {
                script {
                    // extract repo name from URL
                    env.REPO_NAME = REPO_URL.tokenize('/').last().replace('.git','')
                    echo "Repo Name: ${env.REPO_NAME}"
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
