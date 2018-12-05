pipeline {
  agent any
  stages {
    stage('checkout code') {
      steps {
        git(url: 'https://github.com/avinashsi/javaproject.git', branch: 'master', poll: true, credentialsId: '767b72ca-de52-4da5-9b39-282425c14dcd')
      }
    }
    stage('compile code') {
      steps {
        sh '/usr/maven/bin/mvn clean package'
      }
    }
    stage('Get The DockerFile') {
      steps {
        git(url: 'https://github.com/avinashsi/dockerfile.git', branch: 'master', credentialsId: '767b72ca-de52-4da5-9b39-282425c14dcd')
      }
    }
    stage('Add the war file') {
      parallel {
        stage('Add the war file') {
          steps {
            sh '''cat >> /var/lib/jenkins/workspace/dockerfile_master/Tomcat8_Dockerfile/Dockerfile <<EOL
ADD /var/lib/jenkins/workspace/dockerfile_master/target/trucks.war /home/tomcat/webapps/
EOL'''
          }
        }
        stage('Copy the war file') {
          steps {
            sh 'sudo cp /var/lib/jenkins/workspace/dockerfile_master/target/trucks.war /var/lib/jenkins/workspace/dockerfile_master/Tomcat8_Dockerfile'
          }
        }
      }
    }
    stage('Docker Image Build') {
      steps {
        sh 'sudo docker build -t  tomcat":$BUILD_NUMBER" /var/lib/jenkins/workspace/dockerfile_master/Tomcat8_Dockerfile'
      }
    }
  }
}