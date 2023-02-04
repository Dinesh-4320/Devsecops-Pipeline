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
    
    stage ('check-git-secrets'){
      steps{
        sh '''
              rm trufflehog || true
              docker run gesellix/trufflehog --json https://github.com/Dinesh-4320/Devsecops-Pipeline.git > trufflehog
              cat trufflehog
           '''
      }
    }
   
    stage('Source Composition Analysis'){
	steps{
	 sh ''' 
              rm owasp* || true
	      wget https://raw.githubusercontent.com/Dinesh-4320/Devsecops-Pipeline/master/owasp-dependency-check.sh
	      chmod +x owasp-dependency-check.sh
	      ./owasp-dependancy-check.sh
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
