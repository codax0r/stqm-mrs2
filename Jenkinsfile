pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v $HOME/.m2:/root/.m2'
    }
  }

  tools {
    maven 'apache-maven-3.6.0'
    jdk 'openjdk-11'
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
      //agent { // Das Ganze Docker zeugs geht auch pro stage
      //  docker {
      //    image 'maven:3-alpine'
      //    args '-v $HOME/.m2:/root/.m2'
      //  }
      //}

      steps {
        sh 'mvn package'
      }
    }

    stage('Integration Test') {
      steps {
        sh 'mvn verify' 
      }
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
