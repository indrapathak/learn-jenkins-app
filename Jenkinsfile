pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm config set cache .npm
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}