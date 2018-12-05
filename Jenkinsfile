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
        sh 'mvn clean package'
      }
    }
    stage('copying File Locally') {
      steps {
        sh 'cp /var/lib/jenkins/workspace/mvnTest/target/trucks.war /home/vagrant/files/app_data/application1/'
      }
    }
    stage('Get The DockerFile') {
      steps {
        git(url: 'https://github.com/avinashsi/dockerfile.git', branch: 'master', credentialsId: '767b72ca-de52-4da5-9b39-282425c14dcd')
      }
    }
    stage('Add the war file') {
      steps {
        sh '''cat >> /dockerfile/Tomcat8_Dockerfile/Dockerfile <<EOL
ADD /home/vagrant/files/app_data/application1/trucks.war /home/tomcat/webapps/
EOL'''
      }
    }
    stage('Docker Image Build') {
      steps {
        sh 'docker build -t  tomcat":$BUILD_NUMBER" /dockerfile/Tomcat8_Dockerfile/'
      }
    }
  }
  environment {
    PATH = '$PATH:/usr/maven/bin'
  }
}