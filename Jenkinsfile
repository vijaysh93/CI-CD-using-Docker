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
				sh '''
					#!/bin/bash
					echo "verfying previous builds status"
					builds=`docker ps -q`
					if [$builds -ne 0 ];then
					echo "stopping previous builds"
					docker stop $builds
					else
					echo "running new build"
					docker run -d --rm -p 8001:8080 vijaysh93/samplewebapp:latest
					echo "success build"
					fi
					'''
            
 		
            }
        }
 
    }
	}
    
