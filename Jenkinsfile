pipeline {
	environment { 
        registry = "frickson/webapp" 
        registryCredential = 'frickson'  
    }
    agent any
    tools{
       maven 'Maven' 
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
              
                sh 'docker build -t webapp:latest .' 
                sh 'docker tag webapp frickson/webapp:latest'
                //sh 'docker tag webapp frickson/webapp:$BUILD_NUMBER'
               
          }
        }
     
	stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "frickson", url: "" ]) {
          sh  'docker push frickson/webapp:latest'
        //  sh  'docker push frickson/webapp:$BUILD_NUMBER' 
        }
                  
          }
        }
     

      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 frickson/webapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@110.238.110.156 run -d -p 8003:8080 frickson/webapp"
 
            }
        }
    }
	}
    
