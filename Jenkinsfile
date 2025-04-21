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
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: 'localhost:8082',
          groupId: 'QA',
          version: version,
          repository: 'SimpleJSPapp-release',
          credentialsId: "c272254f-d0ce-430c-8b4d-89e074005104",
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









