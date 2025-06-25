pipeline {
  agent any

  tools {
    nodejs 'node-22' // oder der Name, den du in Jenkins eingegeben hast
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
