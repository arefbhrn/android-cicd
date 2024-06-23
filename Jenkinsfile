pipeline {
    agent any

    environment {
        // Telegram configure
        TOKEN = credentials('telegramToken')
        CHAT_ID = credentials('telegramChatId')

        // Telegram Message Pre Build
        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_MESSAGE = sh(returnStdout: true, script: "git log -n 1 --format=%s ${GIT_COMMIT}").trim()
        GIT_AUTHOR = sh(returnStdout: true, script: "git log -n 1 --format=%ae ${GIT_COMMIT}").trim()
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
        GIT_INFO = "Branch(Version): ${GIT_BRANCH}\nLast Message: ${GIT_MESSAGE}\nAuthor: ${GIT_AUTHOR}\nCommit: ${GIT_COMMIT_SHORT}"
        TEXT_BREAK = "--------------------------------------------------------------"
        TEXT_PRE_BUILD = "${TEXT_BREAK}\n${GIT_INFO}\n${JOB_NAME} is Building"

        // Telegram Message Success and Failure
        TEXT_SUCCESS_BUILD = "${JOB_NAME} is Success"
        TEXT_FAILURE_BUILD = "${JOB_NAME} is Failure"
    }

    stages {
        stage('Pre-Build') {
            steps {
                sh "curl --location --request POST 'https://api.telegram.org/bot${TOKEN}/sendMessage' --form text='${TEXT_PRE_BUILD}' --form chat_id='${CHAT_ID}'"
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        success {
            script{
                sh "curl --location --request POST 'https://api.telegram.org/bot${TOKEN}/sendMessage' --form text='${TEXT_SUCCESS_BUILD}' --form chat_id='${CHAT_ID}'"
            }
        }
        failure {
            script{
                sh "curl --location --request POST 'https://api.telegram.org/bot${TOKEN}/sendMessage' --form text='${TEXT_FAILURE_BUILD}' --form chat_id='${CHAT_ID}'"
            }
        }
    }

}