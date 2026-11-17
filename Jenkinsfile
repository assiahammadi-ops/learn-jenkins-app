pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh 'ls -la'
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
                sh 'npm run build'
                sh 'ls -la'
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
                sh 'test -f build/index.html'
                sh 'npm test'
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                NETLIFY_SITE_ID = 'REMPLACE_PAR_TON_SITE_ID'
                NETLIFY_AUTH_TOKEN = credentials('netlify-token')
            }
            steps {
                sh 'npm install netlify-cli'
                sh 'node_modules/.bin/netlify status'
                sh 'node_modules/.bin/netlify deploy --dir=build --prod'
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
