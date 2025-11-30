pipeline {
  agent any

  stages {
    stage('Hello') {
      steps {
          sh '''
            echo 'Hello World without docker'
            ls -la
            touch container-no.txt
          '''
      }
    }
    
    stage('This Runs Docker') {
      agent {
        docker {
          image 'node:18-alpine'
          reuseNode true
        }
      }

      steps {
          sh '''
            echo 'Hello World with docker'
            npm --version
            ls -la
            touch container-yes.txt
          '''
        
      }
    }
  }
}