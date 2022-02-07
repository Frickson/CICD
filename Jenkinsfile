pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/Frickson/CICD.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t webapp_1:latest .' 
                sh 'docker tag webapp_1 frickson/webapp_1:latest'
                //sh 'docker tag samplewebapp frickson/webapp_1:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "1", url: "" ]) {
          sh  'docker push frickson/webapp_1:latest'
        //  sh  'docker push frickson/webapp_1:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 frickson/webapp_1"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://root@159.138.122.95 run frickson/webapp_1"
 
            }
        }
    }
	}
    
