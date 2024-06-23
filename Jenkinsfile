pipeline {
    agent any

    environment {
        REGISTRY = 'com.arefdev.cicd'
    }

    stages {
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
    }

}