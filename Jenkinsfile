#!groovy

pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t gauthamdharsan/poc:latest .'
      }
    }
  }
}
        stage ('Run') {
             docker.image('gauthamdharsan/poc:latest').run('-p 4444:4444 -h gateway --name gateway --link discovery --link accounts --link customer')
        }
     
