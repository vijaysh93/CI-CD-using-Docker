pipeline {
    agent any
	
	
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/vijaysh93/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp vijaysh93/samplewebapp:latest'
                //sh 'docker tag samplewebapp vijaysh93/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "vijaysh93", url: "" ]) {
     
		sh  'docker push vijaysh93/samplewebapp:latest'
        //  sh  'docker push vijaysh93/samplewebapp:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{	
				sh "docker stop tomcat_test"
				sh "docker run -d --rm -p 8001:8080 --name tomcat_test vijaysh93/samplewebapp:latest"
            
             }
        }
 
    }
	}
    
