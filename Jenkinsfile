pipeline {
	agent any
/*	tools {
	  maven "Maven"
		jdk "JDK"
	}*/
	stages {
		/*stage('Initialize'){
			steps{
				echo "PATH = ${M2_HOME}/bin:${PATH}"
				echo "M2_HOME = /opt/maven"
						}
	  }
		stage('Build') {
			steps {
				dir("/opt/jenkins/rock-paper-scissors") {
					sh 'mvn -B -DskipTests clean package'
				}
			}
		}*/
		stage('VariableDefinition') {
			steps{
				/*script{
					dir("/opt/jenkins/rock-paper-scissors/target"){
						//def artifact_name = sh script: 'find . -type f -name "*.jar" | head -n 1', returnStdout: true
						sh script: "find . -type f -name '*.jar' | head -n 1 | cut -c 3- | rev | cut -c 1-| rev | tr -d '\n' > result"
					}	
				}*/
				script{
					//artifact_name = readFile('/opt/jenkins/rock-paper-scissors/target/result') 
					def artifact_name
					def pr
					//project_name = artifact_name.substring(0, 3)
					project_name = repo_name.reverse().take(3).reverse().toLowerCase() 
					println project_name
					//repo_name= "M-" + project_name.toUpperCase()
					println repo_name
					
				}
			}
		}
		/*stage ('Push'){
			steps{
				dir("/opt/jenkins/rock-paper-scissors/target"){
					withCredentials([usernamePassword(credentialsId: 'cf6a9544-f46a-44d6-a9e1-33f8c9c02b6c', passwordVariable: 'password_nexus', usernameVariable: 'username_nexus')]) {
						sh ("curl -v -u \$username_nexus:\$password_nexus --upload-file ${artifact_name} http://172.29.16.32:8081/repository/${repo_name}/${artifact_name}")
					}
				}	
			}
		}*/
		
		
		stage('deploy') {
		    emailext mimeType: 'text/html',
                    subject: "[Jenkins]${currentBuild.fullDisplayName}",
                    to: "s.fois@vargroup.it",
                    body: '''<a href="${BUILD_URL}input">click to approve</a>'''
			
		    input {
			message "Should we continue?"
			ok "Yes"
		    }
		    steps {
			sh "echo 'describe your deployment' "
		    }
		//}
		
		
		//stage ('Ansible'){
			steps{
				script {
					try {
						ansibleTower(
							extraVars: """---
artifact_name: "${artifact_name}"
project_name: "${project_name}"
repo_name: "${repo_name}"
""",
							jobTemplate: 'Deploy Jenkins',
							jobType: 'run',
							throwExceptionWhenFail: true,
							towerCredentialsId: 'f3330343-d0b8-4e32-9d51-6883f312b1ad',
							towerLogLevel: 'false',
							towerServer: 'Ansible - POC'
						)
					}catch (Exception e) {
						echo 'culo'
					}
				}	
			}
		}
	}
} 



