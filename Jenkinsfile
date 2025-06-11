pipeline {
    agent any
    
    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                checkout scm
            }
        }

        stage('AWS') {
            agent {
                docker { 
                    image 'amazon/aws-cli'
                    args "--entrypoint=''"
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-jenkins-creds', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        aws s3 ls
                    '''
                }
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

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
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
                    test -f build/index.html
                    npm test
                '''
            }
        }
    } 
}