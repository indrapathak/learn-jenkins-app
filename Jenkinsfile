pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    args '-v $HOME/.npm:/home/node/.npm'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}