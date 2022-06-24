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
  }
}
