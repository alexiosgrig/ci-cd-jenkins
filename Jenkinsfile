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
            steps {
                sh '''
                    docker run -d --name vite-builder -w /app node:20-alpine sleep 3600
                    docker cp my-react-app/. vite-builder:/app/
                    docker exec vite-builder npm install
                    docker exec vite-builder npm run build
                    docker cp vite-builder:/app/dist ./dist
                    docker rm -f vite-builder
                '''
            }
        }
    }

    post {
        always {
            sh 'docker rm -f vite-builder || true'
        }
        success {
            echo 'Build completed successfully! Your Vite app is ready.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}