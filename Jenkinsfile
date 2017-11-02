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
        sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
      }
    }
      
    stage('Running on Centos') { 
      agent { label 'CentOS'}
      steps {
        sh "wget http://hultanu4.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    
      
    stage('Test on Debian') { 
      agent { docker 'openjdk:8u121-jre'}
      steps {
        sh "wget http://hultanu4.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
      }
    }
    
    stage('Promote to green') { 
      agent { label 'CentOS'}
      when {
        branch 'master' 
      }
      steps {
        sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
      }
    }
    
    stage('Promote Development branch to MASTER') { 
      agent { label 'CentOS'}
      when {
        branch 'development' 
      }
      steps {
        echo "Stashing any local changes"
        sh 'git stash'
        echo 'Checking out development branch'
        sh 'git checkout development'
        echo 'checking out master branch'
        sh 'git checkout master'
        echo 'merging into master branch'
        sh 'git merge development'
        echo 'pushing to origin master'
        h 'git push origin master'
      }
    }
    
    
    
  }
 
}