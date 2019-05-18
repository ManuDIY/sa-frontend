pipeline {
  agent any
 
  stages {
    stage('Pre-Build') {
      steps {
		gitlabCommitStatus(name: 'npm install'){
        		sh "npm install"
		}
      }
    }
   stage('Building') {
      steps {
		gitlabCommitStatus(name: 'npm run build'){
        		sh "npm run build"
		}
      }
    }
   stage('Packaging') {
      steps {
		gitlabCommitStatus(name: 'Docker image build'){
        		sh "docker image build --tag ansilh/sa-frontend-${env.BUILD_NUMBER} ."
	}
      }
    }
  }
  post {
     success {
		updateGitlabCommitStatus(name: 'Pipeline', state: 'success')
	}
  }
}
