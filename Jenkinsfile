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
  }
  stages {
    stage('Build Image') {
      steps {
        slackSend(channel: "pipeline", message: "[ANDROID]Job Started! :)", sendAsText: true)
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
    stage('Slark Noti') {
        steps {
            slackSend(channel: "pipeline", message: "Success! :)", sendAsText: true)
        }
    }
  }
  post {
      failure {
          slackSend(channel: "pipeline",color: "danger", message: "Failed! :)", sendAsText: true)
      }
      unstable {
          // slackSend color: "danger", message: "*${env.JOB_NAME}* *${env.BRANCH_NAME}* job is unstable. Unstable means test failure, code violation etc."
          slackSend(channel: "pipeline",color: "danger", message: "Job is unstable![Unstable means test failure, code violation etc] :)", sendAsText: true)
      }
  }
}