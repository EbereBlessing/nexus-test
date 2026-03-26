pipeline {
  agent any
  tools {
    jdk 'jdk8'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build with Nexus') {
      steps {
        configFileProvider([configFile(
          fileId: '2216865e-289d-4504-a2fb-8995a920cc66',
          variable: 'MAVEN_SETTINGS'
        )]) {

          sh '''
          echo "Using managed Maven settings..."

          chmod +x mvnw

          ./mvnw clean package --settings $MAVEN_SETTINGS -DskipTests
          '''
        }
      }
    }
  }
}
