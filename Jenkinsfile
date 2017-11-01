pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building..'
        sh 'ant -f build.xml -v'
      }
    }
  }
  
  post {
    always {
      archive 'dist/*.jar'
    }
  }
    
}