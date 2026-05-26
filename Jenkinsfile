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
                // Μπαίνει στο my-react-app και τρέχει npm install για να δημιουργήσει τα node_modules
                sh 'docker run --rm -v "$(pwd)":/app -w /app/my-react-app node:20 npm install'
            }
        }

        stage('Build') {
            steps {
                // Μπαίνει στο my-react-app και χτίζει το Vite project
                sh 'docker run --rm -v "$(pwd)":/app -w /app/my-react-app node:20 npm run build'
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully! Your Vite app is ready.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}