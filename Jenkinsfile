pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/alexiosgrig/ci-cd-jenkins.git'
            }
        }

        stage('Install & Build') {
            agent {
                dockerContainer {
                    image 'node:20-alpine'
                }
            }
            steps {
                sh '''
                    cd my-react-app
                    npm install
                    npm run build
                '''
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}