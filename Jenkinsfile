pipeline {
  agent any
  environment {
   JENKINS_AVATAR_URL="https://raw.githubusercontent.com/jenkinsci/jenkins/master/war/src/main/webapp/images/headshot.png"
   PUSHED_BY = sh(
		script: "git show -s --format='%an(%ae)'  HEAD",
		returnStdout: true,
		)
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
			sh "env"
	}
      }
    }
  }
  post {
     success {
		rocketSend(
    			attachments: [[
        			title: "${env.gitlabSourceRepoName}",
        			color: 'green',
        			text: 'Build Success  :white_check_mark: ',
        			thumbUrl: '',
        			messageLink: '',
        			collapsed: false,
        			authorName: "Started by the changes from ${PUSHED_BY}",
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
		rocketSend(
                        attachments: [[
        			title: "${env.gitlabSourceRepoName}",
                                color: 'red',
                                text: 'Build failed  :negative_squared_cross_mark: ',
                                thumbUrl: '',
                                messageLink: '',
                                collapsed: false,
                                authorName: "Started by the changes from ${PUSHED_BY}",
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
