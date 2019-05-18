pipeline {
  agent any
  environment {
   JENKINS_AVATAR_URL="https://raw.githubusercontent.com/jenkinsci/jenkins/master/war/src/main/webapp/images/headshot.png"
  } 
  stages {
    stage('Pre-Build') {
      steps {
		gitlabCommitStatus(name: 'npm install'){
			rocketSend(avatar: "$JENKINS_AVATAR_URL", channel: 'sa-project', message: "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
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
		rocketSend(
    			attachments: [[
        			title: 'SA Frontend',
        			color: 'green',
        			text: 'Build Success  :100:',
        			thumbUrl: '',
        			messageLink: '',
        			collapsed: false,
        			authorName: 'Ansil',
        			authorIcon: '',
        			authorLink: '',
        			titleLink: "${env.BUILD_URL}",
        			titleLinkDownload: '',
        			imageUrl: '',
        			audioUrl: '',
        			videoUrl: ''
    			]],
    			channel: 'sa-project',
    			message: '',
    			avatar: "${JENKINS_AVATAR_URL}",
    			failOnError: true,
    			rawMessage: true
		)
		updateGitlabCommitStatus(name: 'Pipeline', state: 'success')
	}
     failure {
		updateGitlabCommitStatus(name: 'Pipeline', state: 'failed')
		rocketSend avatar: "$JENKINS_AVATAR_URL", channel: 'sa-project', message: "sa-frontned *failed* on branch ${env.GIT_BRANC} \nRecent Changes - ${getChangeString(10)}\nBuild: ${BUILD_URL}", rawMessage: true
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
