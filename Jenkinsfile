pipeline {
  environment {
        DOCKERHUB_CREDENTIALS=credentials('haleema-dockerhub')
    }
  agent none
  stages {
    stage('Login to Dockerhub') {
      agent any
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        } 
    
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
            sleep(time: 3, unit: "SECONDS")
            sh 'docker run --privileged -t haleema/docker-rpi'
            sleep(time: 4, unit: "SECONDS")
               } catch (Throwable e) {
                        echo "Caught ${e.toString()}"
                        currentBuild.result = "SUCCESS" //currentBuild.result = 'SUCCESS'
                    }
          }
        }
    }
    stage('On-Edge-receive and save data in data.csv file') {
      options {
                timeout(time: 20, unit: "SECONDS")
            }
   
          agent any
        
          steps {
            script { 
            try {
            sh 'echo "edge1"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge1.git'
            //sh 'docker build -t haleema/docker-edge1:latest .'
            echo "Started stage A"
            sleep(time: 3, unit: "SECONDS")
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge1'
            sleep(time: 2, unit: "SECONDS")
               } catch (Throwable e) {
                        echo "Caught ${e.toString()}"
                        currentBuild.result = "SUCCESS" 
                        //sh 'nano data.csv'             
                    }
              //if (currentBuild.result == 'SUCCESS') {
               // sh 'sleep 2'
                //sh 'nano data.csv'
                //sh 'exit 0' 
              
              //}
              
            }

          }
        }
    
         stage('On-Edge-Data Preprocessing') {
          agent any
          steps {
            sh 'echo "edge3"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge3.git'
            //sh 'docker build -t haleema/docker-edge3:latest .'
            //sh 'sleep 10'
            //sh 'docker stop  haleema/docker-edge1; docker rm  haleema/docker-edge1'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge3'
            sleep(time: 3, unit: "SECONDS")
            sh 'echo "edge2"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge222.git'
            sh 'docker build -t haleema/docker-edge222:latest .'
            //sh 'sleep 10'
            //sh 'docker stop  haleema/docker-edge1; docker rm  haleema/docker-edge1'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge222'
            

          }
        } 
        //stage('On-Edge-Sendeng Processed Data to Cloud') {
          //agent any
          //steps {
            //sh 'echo "edge1"'
            //git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge222.git'
            //sh 'docker build -t haleema/docker-edge222:latest .'
            //sh 'sleep 10'
            //sleep(time: 3, unit: "SECONDS")
            //sh 'docker stop  haleema/docker-edge1; docker rm  haleema/docker-edge1'
            //sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge222'
            

         // }
        //} 
    stage('On-aws-Receive Data from Edge and Save it in data2.csv') {
           options {
                timeout(time: 20, unit: "SECONDS")
            }     
          agent {label 'aws'}
          steps {
            script { 
            try {
            sh 'echo "cloud" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-cloud.git'
            //sh 'docker build -t haleema/docker-cloud:latest .'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-cloud'
            sleep(time: 2, unit: "SECONDS")
               } catch (Throwable e) {
                        echo "Caught ${e.toString()}"
                        currentBuild.result = "SUCCESS" 
                        //sh 'nano data.csv'             
                    }
            } 
          }
        }
    stage('On-aws-Resampling Data and visulization Process') {
          agent {label 'aws'}
      steps {
            sh 'echo "cloud-visualization" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-cloud-visualization.git'
            //sh 'docker build -t haleema/docker-cloud2:latest .'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-cloud2'
            
          }
        }
  
       
    
        
    
  }
}
