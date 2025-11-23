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
                    echo "${DOCKERHUB_PSW}" | docker login -u ${DOCKERHUB_USR} --password-stdin
                    docker push ${IMAGE}:${VERSION}
                    docker push ${IMAGE}:latest
                """
            }
        }
    }

    post{
        always {
            echo "Always runs"
        }

        failure {
            mail to: 'wanyamak884@gmail.com',
                subject: "Jenkins Gmail SMTP Test"
                body: "If you received this email, SMTP failed!"
        }

        success {
            mail to: 'wanyamak884@gmail.com',
                subject: "Jenkins Gmail SMTP Test",
                body: "If you received this email, SMTP is working."
        }
    }
}