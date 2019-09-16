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
      project = "sa-android"
  }
  stages {
    stage('Build Image') {
      steps {
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
  }
}