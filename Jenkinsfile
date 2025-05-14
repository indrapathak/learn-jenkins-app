pipeline {
    agent any
    
    options {
        // This will clean workspace before each build
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                cleanWs() // Clean workspace
                checkout scm // Checkout code
            }
        }
        
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    args '-u 130:138'
                    reuseNode true
                }
            }
            environment {
                HOME = "${WORKSPACE}"
                NPM_CONFIG_CACHE = "${WORKSPACE}/.npm-cache"
            }
            steps {
                sh '''
                    npm install
                    npm run build
                '''
            }
        }
    }
}