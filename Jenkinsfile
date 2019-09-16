pipeline {
  agent any
  stages {
    stage('Build Image') {
      steps {
        sh 'docker build -t ${project}'
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