pipeline {
  agent any
 
  stages {
    stage('Installing dependencies..') {
      steps {
		gitlabCommitStatus(name: 'Prebuild Actions'){
        		sh "npm install"
		}
      }
    }
   stage('Building App..') {
      steps {
        sh "npm run build"
      }
    }
  }
  post {
     success {
	updateGitlabCommitStatus(name: 'Jenkins', state: 'success')
	}
}
