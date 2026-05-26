pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/alexiosgrig/ci-cd-jenkins.git'
            }
        }

        stage('Install') {
            steps {
                sh 'npm ci' 
            }
        }

        stage('Test') {
            steps {
                sh 'npm test -- --watchAll=false'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
    }

    post {
        success {
            echo 'Build completed successfully'
        }

        failure {
            echo 'Build failed'
        }
    }
}