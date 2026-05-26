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
                    # 1. Σήκωσε το container
                    docker run -d --name vite-builder -w /app node:20-alpine sleep 3600
                    
                    # 2. Αντίγραψε τα αρχεία του project
                    docker cp my-react-app/. vite-builder:/app/
                    
                    # 3. Εγκατάσταση βασικών πακέτων
                    docker exec vite-builder npm install
                    
                    # 4. Αναγκαστική εγκατάσταση των Linux Musl native bindings που ζητάει το Vite
                    docker exec vite-builder npm install --save-optional @rolldown/binding-linux-x64-musl lightningcss-linux-x64-musl
                    
                    # 5. Εκτέλεση του Build
                    docker exec vite-builder npm run build
                    
                    # 6. Αντίγραψε τα build αρχεία πίσω στον Jenkins
                    docker cp vite-builder:/app/dist ./dist
                    
                    # 7. Καθάρισε τον container
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