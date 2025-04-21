pipeline{
  agent any
    triggers {
      pollSCM "* * * * * "
    }

  tools {
    maven 'mymaven'
  }

  environment {
    version = "1.0-SNAPSHOT"
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
  // steps {
  //       sh "mvn deploy:deploy-file -DgroupId=QA -DartifactId=SimpleJSPapp -Dversion=${version} -Dpackaging=war -Dfile=target/SimpleJSPapp.war -DrepositoryId=nexus -Durl=http://localhost:8082/repository/SimpleJSPapp-release/"
  //   }
    steps{
    nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: 'localhost:8082',
          groupId: 'QA',
          version: version,
          repository: 'SimpleJSPapp',
          credentialsId: "05750304-b78b-4660-ad87-a2cc09c39d3b",
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









