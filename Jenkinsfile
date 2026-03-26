pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build with Nexus') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'nexus-creds',
          usernameVariable: 'NEXUS_USER',
          passwordVariable: 'NEXUS_PASS'
        )]) {

          sh '''
          echo "Creating Maven settings.xml..."

          cat <<EOF > settings.xml
          <settings>
            <servers>
              <server>
                <id>nexus-repo</id>
                <username>${NEXUS_USER}</username>
                <password>${NEXUS_PASS}</password>
              </server>
            </servers>
          </settings>
          EOF

          echo "Running Maven build..."
          ./mvnw clean package --settings settings.xml
          '''
        }
      }
    }
  }
}
