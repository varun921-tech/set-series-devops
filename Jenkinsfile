pipeline{
  agent any
    triggers {
      pollSCM "* * * * * "
    }

  tools {
    maven 'mymaven'
  }

  environment {
    version = "1.0"
    nexusLogin = "nexusLogin"
  }

  stages{
    stage('Build') {
      steps{
        echo "Building..."
          sh "mvn clean compile -DskipTests"
      }
    }
    stage('Tests') {
      steps{
        echo "Running Tests..."
          sh "mvn test"
      }
    }
  stage('Package') {
    steps{
      echo "Packaging..."
        sh "mvn package -DskipTests"
    }
  }
  stage('Upload to Nexus'){
    steps{
      echo "Using version : ${version}"
      echo "Using nexusLogin : ${nexusLogin}"
      nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: 'localhost:8082',
          groupId: 'QA',
          version: version,
          repository: 'SimpleJSPapp-release',
          credentialsId: "${nexusLogin}",
          artifacts: [
          [artifactId: 'SimpleJSPapp',
          classifier: '',
          file: 'target/SimpleJSPapp.war',
          type: 'war'
          ]
          ]
          )
    }
  }
  stage('Archive Artifacts') {
    steps{
      echo "Archiving..."
        archiveArtifacts artifacts: 'target/*.war', fingerprint:true
    }
  }
}
}









