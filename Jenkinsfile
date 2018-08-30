pipeline{
   environment {
    registry = "ahteshama/practice-demo"
    registryCredential = 'docker'
    dockerImage = ''
  }
  agent any
  
  stages {
  
    stage('Cloning Git') {
      steps {
        git 'http://10.30.5.71/soumyas2/Microservice_POC.git'
        //properties([pipelineTriggers([pollSCM('H/10 * * * *')])])
      }
    }
    stage('Building Project'){
        steps {
            sh 'mvn clean install'
        }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Test image') {
        steps{
            script{
                dockerImage.inside {
                    sh 'echo "Tests passed"'
                }
            }
        }
    }
    stage('Deploy Image') {
        steps{
            script {
              docker.withRegistry( '', registryCredential ) {
                dockerImage.push()
                sh 'echo \'https://hub.docker.com/r/ahteshama/practice-demo/tags/\''
            }
        }
      }
    }
}
}
