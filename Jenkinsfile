pipeline {
    agent {
        label 'mac-mini'
    }
    environment {
        // Fastlane Environment Variables
        PATH = "$HOME/.fastlane/bin:" +
                "/usr/local/bin:" +
                "$PATH"
        LC_ALL = "en_US.UTF-8"
        LANG = "en_US.UTF-8"
        JAVA_HOME = "/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home"
        appName = "Forecastie"
        teamName = "Android"
    }
    stages {
        stage('Run Unit and UI Tests') {
            steps {
                script {
                    try {
                        sh "echo 'JavaHome :' ${env.JAVA_HOME}"
                        slackSend(channel: "pipeline", message: "[${teamName}]${appName} - Job Started! :)", sendAsText: true)
                        sh "fastlane runTests"
                    } catch(exc) {
                      error('There are failed tests.')
                    }                    
                }
            }
        }
        stage('Build application for beta') {
            steps {
                sh "fastlane beta"
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