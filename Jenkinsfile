pipeline {
  agent any
 
  stages {
    stage('Installing dependencies..') {
      steps {
        sh "npm install"
      }
    }
   stage('Building App..') {
      steps {
        sh "npm run build"
      }
    }
  }
}
