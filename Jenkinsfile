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
                    
                    docker exec vite-builder npm install --save-optional @rolldown/binding-linux-x64-musl lightningcss-linux-x64-musl
                    
                    docker exec vite-builder npm run build
                    
                    docker cp vite-builder:/app/dist ./dist
                    
                    docker rm -f vite-builder
                '''
            }
        }

        stage('Deploy to Vercel') {
            steps {
             echo 'Starting deployment to Vercel using Docker...'
             withCredentials([string(credentialsId: 'vercel-auth-token', variable: 'VERCEL_TOKEN')]) {
                  sh '''
                   docker run --rm \
                  -v $(pwd)/dist:/app \
                  -w /app \
                  -e VERCEL_TOKEN="$VERCEL_TOKEN" \
                  node:20-alpine \
                  sh -c "npm install -g vercel && vercel deploy --prod --token \$VERCEL_TOKEN --yes"
            '''
        }
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