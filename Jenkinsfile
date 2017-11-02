pipeline 
{
  agent none
  
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
       
  post {
    success {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
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
    
      
    stage('Test on Debian') { 
      agent { docker 'openjdk:8u121-jre'}
      steps {
        sh "wget http://hultanu4.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    
    stage('Promote to green') { 
      agent { label 'CentOS'}
      steps {
        sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
      }
    }
  }
 
}