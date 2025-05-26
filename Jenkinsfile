pipeline {
    agent any

    environment {
        IMAGE_NAME = "node${env.BRANCH_NAME}"
        VERSION = "v1.0"
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    }

    tools {
        nodejs 'Node 7.8.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${VERSION} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker ps -aq --filter 'name=${IMAGE_NAME}' | xargs -r docker rm -f
                docker run -d -p ${PORT}:3000 --name ${IMAGE_NAME} ${IMAGE_NAME}:${VERSION}
                """
            }
        }
    }
}
