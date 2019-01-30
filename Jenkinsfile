pipeline {
  agent any
  tools {
    maven 'apache-maven-3.6.0'
  }

  // Container, save env
  stages {
    stage('Checkout') {
      steps {
        checkout scm
        sh 'mvn clean'
      }
    }

  // Container, reuse checkout + save env
    stage('Compile + Unit Test') {
      steps {
        sh 'mvn package'
      }
    }

    stage('Integration Test') {
      sh 'mvn verify' 
    }

    stage('Deploy') {
      steps {
        sh 'mkdir -p /var/jenkins_home/download'
        sh 'cp ./target/*.jar /var/jenkins_home/download'
      }
    }
  }

  post {
    always {
      // publish test results
      junit 'target/surefire-reports/*.xml'
    }
  }
}