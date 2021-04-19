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
                sh 'docker tag samplewebapp vijaysh93/samplewebapp:$BUILD_NUMBER'
                //sh 'docker tag samplewebapp vijaysh93/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "vijaysh93", url: "" ]) {
     
		sh  'docker push vijaysh93/samplewebapp:$BUILD_NUMBER'
        //  sh  'docker push vijaysh93/samplewebapp:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{	
				sh "docker ps -f name=tomcat_test -q | xargs --no-run-if-empty docker container stop"
				sh "docker conatiner ps -f name=tomcat_test -q | xargs -r docker container rm"
				sh "docker run -it -d -p 8001:8080 --name tomcat_test vijaysh93/samplewebapp:$BUILD_NUMBER"
            
             }
        }
 
    }
	}
    
