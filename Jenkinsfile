pipeline {
    options { 
        buildDiscarder(logRotator(numToKeepStr: '5'))
        //discard old builds to reduce disk usage
    }
    agent {
        kubernetes {
        label 'spot-pod'
        yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: maven
      image: maven:3.6.3-adoptopenjdk-11
      command:
      - cat
      tty: true
    - name: aws-cli
      image: amazon/aws-cli:latest
      command:
      - cat
      tty: true
  nodeSelector:
    partition: spot-agents
  tolerations:
    - key: partition
      operator: Equal
      value: spot-agents
      effect: NoSchedule
'''
        }
    }
  stages {
    stage('Test') {
      agent { label 'nodejs-app' }
      steps {
        checkout scm
        container('nodejs') {
          echo 'Hello World!'   
          sh 'node --version'
        }
      }
    }
    stage('Build and Push Image') {
      when {
         beforeAgent true
         branch 'master'
      }
      steps {
         echo "TODO - build and push image"
      }
    }
  }
}
