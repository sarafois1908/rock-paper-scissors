pipeline {
	agent any
	tools {
	  maven "Maven"
		jdk "JDK"
	}
	stages {
		stage('Initialize'){
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
		}
		stage('VariableDefinition') {
			steps{
				script{
					dir("/opt/jenkins/rock-paper-scissors/target"){
						//def artifact_name = sh script: 'find . -type f -name "*.jar" | head -n 1', returnStdout: true
						sh script: "find . -type f -name '*.jar' | head -n 1 | cut -c 3- | rev | cut -c 1-|rev > result"
					}	
				}
				script{
					artifact_name = readFile('/opt/jenkins/rock-paper-scissors/target/result') 
				}
			}
		}
		stage ('Push'){
			steps{
				dir("/opt/jenkins/rock-paper-scissors/target"){
					withCredentials([usernamePassword(credentialsId: 'cf6a9544-f46a-44d6-a9e1-33f8c9c02b6c', passwordVariable: 'password_nexus', usernameVariable: 'username_nexus')]) {
						sh "curl -v -u \${username_nexus}:\${password_nexus} --upload-file \$artifact_name http://172.29.16.32:8081/repository/M-PRV/\$artifact_name"
					//echo "$artifact_name"
					}
				}	
			}
		}
	}
} 



