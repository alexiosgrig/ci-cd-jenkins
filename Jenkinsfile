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
                // Αλλάξαμε το 'docker' σε 'dockerContainer' όπως ζήτησε ο Jenkins
                dockerContainer {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                // Μπαίνουμε καθαρά στον φάκελό σου και τρέχουμε τις εντολές
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