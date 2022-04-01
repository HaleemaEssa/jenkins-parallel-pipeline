pipeline {
  environment {
        DOCKERHUB_CREDENTIALS=credentials('haleema-dockerhub')
    }
  agent none
  stages {
    stage('Git-clone') {
      parallel {
        stage('On-Edge1') {
          agent any
          steps {
            sh 'echo "edge1"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge1.git'
            sh 'docker build -t haleema/docker-edge1:latest .'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge1'

          }
        }
        stage('On-Edge2') {
          agent any
          steps {
            sh 'echo "edge1"'
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-edge2.git'
            sh 'docker build -t haleema/docker-edge2:latest .'
            

          }
        } 
        stage('On-RPI') {
          agent {label 'linuxslave1'}
          steps {
            sh 'echo "rpi" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/first_jenkins_project.git'
            sh 'docker build -t haleema/docker-rpi:latest .'
            sh 'docker run --privileged -t haleema/docker-rpi'
          }
        }
        stage('On-aws') {
          agent {label 'aws'}
          steps {
            sh 'echo "cloud" '
            git branch: 'main', url: 'https://github.com/HaleemaEssa/jenkins-cloud.git'
            sh 'docker build -t haleema/docker-cloud:latest .'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge2'
          }
        }
      }
  }
    stage('On-Edge2') {
          agent any
          steps {
            sh 'docker stop  haleema/docker-edge1; docker rm  haleema/docker-edge1'
            sh 'docker run -v "${PWD}:/data" -t haleema/docker-edge2'
          }
    }
    stage('Login to Dockerhub') {
      parallel {
        stage('On-Edge1') {
          agent any
          steps {
            sh 'echo "rn-cloud" '
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }
        stage('On-Edge2') {
          agent any
          steps {
            sh 'echo "edge1-run" '
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }
        stage('On-RPI') {
          agent {label 'linuxslave1'}
          steps {
            sh 'echo "run-pi" '
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }
        stage('On-Cloud') {
          agent {label 'aws'}
          steps {
            sh 'echo "run-pi" '
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
        }
        } 
      }
    
  }
}
