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
                // Στοχεύουμε στο /app/my-react-app επειδή εκεί είναι το package.json
                sh 'docker run --rm -v "$(pwd)":/app -w /app/my-react-app node:20 npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm -v "$(pwd)":/app -w /app/my-react-app node:20 npm test -- --watchAll=false'
            }
        }

        stage('Build') {
            steps {
                sh 'docker run --rm -v "$(pwd)":/app -w /app/my-react-app node:20 npm run build'
            }
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