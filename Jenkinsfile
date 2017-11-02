pipeline 
{
  agent any
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
  }
  
  
  stages {
    stage('Unit Tests') {
      steps {
        echo 'Testing..'
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('Build') {
      steps {
        echo 'Building..'
        sh 'ant -f build.xml -v'
      }
    }
  }
  
  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
  
}