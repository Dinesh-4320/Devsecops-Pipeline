pipeline{
  agent any
  tools{
    maven 'Maven'
  }
  
  stages{
    stage ('initialize') {
      steps{
        sh '''
                  echo "PATH=${PATH}"
                   echo "M2_HOME=${M2_HOME}"
           '''
      }
    }
    
    stage ('Build') {
      steps{
        sh 'mvn clean package'
      }
    }
    
    stage('Deploy-to-Tomcat'){
      steps{
        sshagent(['tomcat']){
        sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@65.0.99.171:/home/ubuntu/prod/apache-tomcat-11.0.0-M1/webapps/webapp.war' 
       }
    }
  }
}
}
