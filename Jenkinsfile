pipeline {
    agent any

    // loads secrets before any stages run
    environment {
        MY_API_KEY = credentials('my-api-key')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Jenkins has detected a change in Github!'
                echo 'Building...'
            }
        }
        stage('Deploy') {
            steps {
                // automatically masks the secret in the logs
                sh 'echo "Deployiing with API Key: $MY_API_KEY"'
            }
        }
    }
}