pipeline 
{
  agent none
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
  }
  
  
  stages {
    stage('Unit Tests') {
      agent { label 'CentOS'}
      steps {
        echo 'Testing..'
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }
    }
    stage('Build') { 
      agent { label 'CentOS'}
      steps {
        echo 'Building..'
        sh 'ant -f build.xml -v'
      }
    }
    
    stage('deploy') { 
      agent { label 'CentOS'}
      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
      }
    }
      
    stage('Running on Centos') { 
      agent { label 'CentOS'}
      steps {
        sh "wget http://hultanu4.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }
  }
  
  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
  
}