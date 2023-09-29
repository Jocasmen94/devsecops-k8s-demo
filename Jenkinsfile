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
        script {
          // Load Docker Hub credentials
          def dockerHubCredentials = credentials('docker-hub-credentials') // Use the ID you provided

          // Authenticate with Docker Hub
          docker.withRegistry('https://index.docker.io/v1/', dockerHubCredentials) {
            sh 'printenv'
            sh 'docker build -t siddharth67/numeric-app:"$GIT_COMMIT" .'
            sh 'docker push siddharth67/numeric-app:"$GIT_COMMIT"'
          }
        }
      }
    }
  }
}
