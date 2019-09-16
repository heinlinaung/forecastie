pipeline {
  agent any
  environment {
      // Fastlane Environment Variables
      PATH = "$HOME/.fastlane/bin:" +
              "/usr/local/bin:" +
              "$PATH"
      LC_ALL = "en_US.UTF-8"
      LANG = "en_US.UTF-8"
      project = "sa-android"
      appName = "Sample App"
      teamName = "ANDROID"
  }
  stages {
    stage('Build Image') {
      steps {
        slackSend(channel: "pipeline", message: "[${teamName}]${appName} - Job Started! :)", sendAsText: true)
        sh 'docker build -t ${project} .'
      }
    }
    stage('Run application test') {
      steps {
        sh 'env >> .env'
        sh 'docker run --env-file .env --rm ${project} ./gradlew test'
        sh 'rm -rf .env'
      }
    }
    stage('Deploy application') {
      steps {
        sh 'env >> .env'
        sh 'docker run --env-file .env --rm ${project} ./gradlew clean build assembleRelease crashlyticsUploadDistributionRelease'
        sh 'rm -rf .env'
      }
    }
  }
  post {
      success {
          slackSend(channel: "pipeline", message: "[${teamName}]${appName} - Success! :)", sendAsText: true)
      }
      failure {
          slackSend(channel: "pipeline",color: "danger", message: "[${teamName}]${appName} - Failed! :)", sendAsText: true)
      }
  }
}