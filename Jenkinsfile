pipeline {
  agent any

  environment {
    NETLIFY_SITE_ID = '4d5f0196-53c6-4960-9589-16cd03604ee6'
    NETLIFY_AUTH_TOKEN = credentials('NETLIFY_TOKEN')
  }

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
            echo 'Hello World with docker'
            npm --version
            node --version
            ls -la
            npm ci
            npm run build
            ls -la
          '''
        
      }
    }

        stage('Install netlify') {
      agent {
        docker {
          image 'node:18-alpine'
          reuseNode true
        }
      }

      steps {
          sh '''
            echo '@@@Down from here will install NETLIFY@@@'
            npm install netlify-cli
            echo '@@@The netlify version is: '
            node_modules/.bin/netlify --version
            node_modules/.bin/netlify status
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