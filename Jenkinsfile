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
  // stage('Upload to Nexus'){
  // // steps {
  // //       sh "mvn deploy:deploy-file -DgroupId=QA -DartifactId=SimpleJSPapp -Dversion=${version} -Dpackaging=war -Dfile=target/SimpleJSPapp.war -DrepositoryId=nexus -Durl=http://localhost:8082/repository/SimpleJSPapp-release/"
  // //   }
  //   steps{
  //   nexusArtifactUploader(
  //         nexusVersion: 'nexus3',
  //         protocol: 'http',
  //         nexusUrl: 'localhost:8082',
  //         groupId: 'QA',
  //         version: version,
  //         repository: 'SimpleJSPapp-release',
  //         credentialsId: "05750304-b78b-4660-ad87-a2cc09c39d3b",
  //         artifacts: [
  //         [artifactId: 'SimpleJSPapp',
  //         classifier: '',
  //         file: 'target/SimpleJSPapp.war',
  //         type: 'war'
  //         ]
  //         ]
  //         )
  //   }
  // }
  stage('Archive Artifacts') {
    steps{
      echo "Archiving..."
        archiveArtifacts artifacts: 'target/*.war', fingerprint:true
    }
  }
      stage('Deploy to Tomcat'){
      steps{
        echo "Deploying..."
        deploy adapters: [tomcat9(credentialsId: 'tomcat_deployer', path: '', url: 'http://ec2-13-49-70-227.eu-north-1.compute.amazonaws.com:8080/')], contextPath: null, war: 'target/*.war'
      }
    }

}
}









