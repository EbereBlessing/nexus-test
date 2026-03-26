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

cat > settings.xml <<EOF
<settings>
  <mirrors>
    <mirror>
      <id>nexus-repo</id>
      <mirrorOf>*</mirrorOf>
      <url>http://16.170.234.97:8081/repository/maven-public/</url>
    </mirror>
  </mirrors>

  <servers>
    <server>
      <id>nexus-repo</id>
      <username>${NEXUS_USER}</username>
      <password>${NEXUS_PASS}</password>
    </server>
  </servers>
</settings>
EOF

echo "Fixing mvnw permissions..."
chmod +x mvnw

echo "Running Maven build..."
./mvnw clean package --settings settings.xml
'''
        }
      }
    }
  }
}
