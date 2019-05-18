pipeline {
  agent any
 
  stages {
    stage('Pre-Build') {
      steps {
		gitlabCommitStatus(name: 'Prebuild Actions'){
        		sh "npm install"
		}
      }
    }
   stage('Building') {
      steps {
        	sh "npm run build"
      }
    }
   stage('Packaging') {
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
