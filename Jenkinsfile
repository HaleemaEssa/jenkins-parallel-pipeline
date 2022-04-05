pipeline {
  environment {
        DOCKERHUB_CREDENTIALS=credentials('haleema-dockerhub')
    }
  agent none
  stages {
    stage('On-RPI') {
      options {
                timeout(time: 60, unit: "SECONDS")
            }
          agent {label 'linuxslave1'}
          steps {
             script { 
            try {
            sh 'echo "rpi" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/first_jenkins_project.git'
            //sh 'docker build -t haleema/docker-rpi:latest .'
            sh 'docker run --privileged -t haleema/docker-rpi'
               } catch (Throwable e) {
                        echo "Caught ${e.toString()}"
                        currentBuild.result = "SUCCESS" 
                    }
          }
        }
    }
    stage('On-Edge1&pi') {
      options {
                timeout(time: 60, unit: "SECONDS")
            }
   
          agent any
        
          steps {
            script { 
            try {
            sh 'echo "edge1"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge1.git'
            //sh 'docker build -t haleema/docker-edge1:latest .'
            echo "Started stage A"
            //sleep(time: 60, unit: "SECONDS")
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge1'
            //sleep(time: 3, unit: "SECONDS")
               } catch (Throwable e) {
                        echo "Caught ${e.toString()}"
                        currentBuild.result = "SUCCESS" 
                    }
              
            }

          }
        }
         
        
    stage('On-aws') {
          agent {label 'aws'}
          steps {
            sh 'echo "cloud" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-cloud.git'
            //sh 'docker build -t haleema/docker-cloud:latest .'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-cloud'
            
          }
        }
  
        stage('On-Edge2') {
          agent any
          steps {
            sh 'echo "edge1"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge2.git'
            //sh 'docker build -t haleema/docker-edge2:latest .'
            //sh 'sleep 10'
            //sh 'docker stop  haleema/docker-edge1; docker rm  haleema/docker-edge1'
            //sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge2'
            

          }
        }
    
        
    
  }
}
