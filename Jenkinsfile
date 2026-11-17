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
                sh 'npm config set fetch-retries 5'
                sh 'npm config set fetch-retry-mintimeout 20000'
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
            post {
                always {
                    junit 'test-results/junit.xml'
                }
            }
        }
    }
}
