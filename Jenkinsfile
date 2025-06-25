pipeline {
  agent any

  tools {
    nodejs 'node-22' 
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'npm ci'
        sh 'npm run build'
      }
    }
  }
}
