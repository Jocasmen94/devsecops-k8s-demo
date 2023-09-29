pipeline {
  agent any

  stages {
    stage('Build Artifact') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }

    stage('Docker Build and Push') {
      steps {
        docker.withTool('docker') {
          sh 'docker build -t siddharth67/numeric-app:"$GIT_COMMIT" .'
          sh 'docker push siddharth67/numeric-app:"$GIT_COMMIT"'
        }
      }
    }
  }
}