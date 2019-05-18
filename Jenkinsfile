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
   stage('Packaging container..') {
      steps {
        	sh "docker image build --tag ansilh/sa-frontend-${env.BUILD_NUMBER} ."
      }
    }
  }
  post {
     success {
		updateGitlabCommitStatus(name: 'Jenkins', state: 'success')
	}
  }
}
