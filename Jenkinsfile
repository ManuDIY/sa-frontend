pipeline {
    agent any
    environment {
        JENKINS_AVATAR_URL="https://raw.githubusercontent.com/jenkinsci/jenkins/master/war/src/main/webapp/images/headshot.png"
    }
    stages {
        stage('Pre-Build') {
            steps {
                gitlabCommitStatus(name: 'NPM install'){
                    rocketSend(
                        attachments: [[
                            title: "${env.gitlabSourceRepoName}",
                            color: 'yellow',
                            text: "In-progress :arrows_counterclockwise:\nRecent Changes - \n${getChangeString(10)}",
                            thumbUrl: '',
                            messageLink: '',
                            collapsed: false,
                            authorName: "Build job ${env.BUILD_NUMBER} started",
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
                    sh "npm install"
                }
            }
        }
        stage('Build') {
            steps {
                gitlabCommitStatus(name: 'NPM build'){
                    sh "npm run build"
                }
            }
        }
        stage('Package and Stage') {
            steps {
                gitlabCommitStatus(name: 'Docker build and Push'){
                    script {
                        docker.withRegistry('https://registry.linxlabs.com:5000', 'docker-cred'){
                                def customImage = docker.build("sa-frontend:${env.BUILD_ID}")
                                customImage.push()
                        }
                    }
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
                    text: "Success  :white_check_mark: \nRecent Changes - \n${getChangeString(10)}",
                    thumbUrl: '',
                    messageLink: '',
                    collapsed: false,
                    authorName: "Build job ${env.BUILD_NUMBER} completed",
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
                    text: "Failed  :negative_squared_cross_mark: \nRecent Changes - ${getChangeString(10)}",
                    thumbUrl: '',
                    messageLink: '',
                    collapsed: false,
                    authorName: "Build job ${env.BUILD_NUMBER} failed",
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

