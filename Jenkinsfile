pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t simple-node-app:${BUILD_NUMBER} .'
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                docker run -d --name test-app -p 3000:3000 simple-node-app:${BUILD_NUMBER}
                sleep 10
                curl http://localhost:3000/health
                docker stop test-app
                docker rm test-app
                '''
            }
        }
    }
}
