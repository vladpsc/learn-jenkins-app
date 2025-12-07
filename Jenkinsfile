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
    
    stage('Run build') {
      agent {
        docker {
          image 'node:18-alpine'
          reuseNode true
        }
      }

      steps {
          sh '''
            npm -install  netlify-cli
            netlify --version
          '''
        
      }
    }

    stage('Test build'){
      agent {
        docker {
          image 'node:18-alpine'
          reuseNode true
        }
      }

      steps {
        sh '''
          test -f build/index.html
          npm test
        '''
      }

      post {
        always {
          junit 'test-results/junit.xml'
        }
      }
    }

  }
}