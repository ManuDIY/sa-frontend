pipeline {
  agent any
  environment {
   JENKINS_AVATAR_URL = "https://raw.githubusercontent.com/jenkinsci/jenkins/master/war/src/main/webapp/images/headshot.png"
  } 
  stages {
    stage('Pre-Build') {
      steps {
		gitlabCommitStatus(name: 'npm install'){
        		sh "npm install"
		}
      }
    }
   stage('Build') {
      steps {
		gitlabCommitStatus(name: 'npm run build'){
        		sh "npm run build"
		}
      }
    }
   stage('Package') {
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
		rocketSend avatar: "$JENKINS_AVATAR_URL", channel: '#sa-frontend', message: "sa-frontned *compiled* on branch ${env.BRANCH_NAME} \nRecent Changes - ${getChangeString(10)}\nBuild: ${BUILD_URL}", rawMessage: true
	}
  }
}
@NonCPS
def getChangeString(maxMessages) {
    MAX_MSG_LEN = 100
    COMMIT_HASH_DISPLAY_LEN = 7
    def changeString = ""

    def changeLogSets = currentBuild.changeSets


    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length && i + j < maxMessages; j++) {
            def entry = entries[j]
            truncated_msg = entry.msg.take(MAX_MSG_LEN)
            commitHash = entry.commitId.take(COMMIT_HASH_DISPLAY_LEN)
            changeString += "${commitHash}... - ${truncated_msg} [${entry.author}]\n"
        }
    }

    if (!changeString) {
        changeString = " There have not been changes since the last build"
    }
    return changeString
}
