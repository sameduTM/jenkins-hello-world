pipeline {
    agent any

    environment {
        IMAGE = "samedutm/node-app"
        VERSION = "v${env.BUILD_NUMBER}"
        DOCKERHUB = credentials('dockerhub-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sameduTM/jenkins-hello-world.git'
            }
        }

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${IMAGE}:${VERSION} .
                    docker tag ${IMAGE}:${VERSION} ${IMAGE}:latest
                """
            }
        }

        stage('PUSH IMAGE') {
            steps {
                sh """
                    echo "${DOCKERHUB_PSW}" | docker login -u "${DOCKERHUB_USR}" --password-stdin
                    docker push ${IMAGE}:${VERSION}
                    docker push ${IMAGE}:latest
                """
            }
        }
    }

    post {
        always {
            echo "Always runs"
        }

        failure {
            emailext(
                to: 'wanyamak884@gmail.com',
                subject: "❌ Jenkins Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                    Build failed!

                    Job: ${env.JOB_NAME}
                    Build Number: ${env.BUILD_NUMBER}
                    URL: ${env.BUILD_URL}

                    Check Jenkins console output for details.
                """,
                mimeType: 'text/plain'
            )
        }

        success {
            mail to: 'wanyamak884@gmail.com',
                 subject: "Jenkins Gmail SMTP Test SUCCESS",
                 body: "Good news! Your Jenkins Gmail SMTP works perfectly!"
        }
    }
}
