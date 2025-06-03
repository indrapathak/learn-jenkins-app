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

          stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

         stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify --version
                '''
            }
        }
// This stage for playwright based end to end test 
        //   stage('E2E') {
        //     agent {
        //         docker {
        //             // docker image of playright
        //             image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
        //             reuseNode true
        //         }
        //     }

        //     steps {
        //         sh '''
        //             npm install serve
        //             node_modules/.bin/serve -s build &
        //             sleep 10
        //             npx playwright test
        //         '''
        //     }
        // }
    }
// For generating Junit test report
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}