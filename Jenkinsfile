pipeline {
  agent none
  stages {
    stage('build') {
      parallel {
        stage('linux-armv6') {
          agent any
          steps {
            sh 'echo "edge1"'
          }
        }
        stage('darwin-amd64') {
          agent {label 'linuxslave1'}
          steps {
            sh 'echo "rpi" '
          }
        }
        stage('linux-amd64') {
          agent {label 'aws'}
          steps {
            sh 'echo "cloud" '
          }
        }
      }
    }
    stage('run') {
      parallel {
        stage('linux-armv6') {
          agent {label 'aws'}
          steps {
            sh 'echo "rn-cloud" '
          }
        }
        stage('darwin-amd64') {
          agent any
          steps {
            sh 'echo "edge1-run" '
          }
        }
        stage('linux-amd64') {
          agent {label 'linuxslave1'}
          steps {
            sh 'echo "run-pi" '
          }
        }
      }
    }
  }
}
