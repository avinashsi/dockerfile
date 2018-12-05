pipeline {
  agent any
  stages {
    stage('chekout Java code') {
      steps {
        git(url: 'https://github.com/avinashsi/javaproject.git', branch: 'master', poll: true)
      }
    }
    stage('compile maven code') {
      steps {
        export PATH = $PATH:/usr/maven/bin
        mvn clean package
      }
    }
    stage('copying File Locally') {
      steps {
        sh 'cp /var/lib/jenkins/workspace/mvnTest/target/trucks.war /home/vagrant/files/app_data/application1/'
      }
    }
  }
}
