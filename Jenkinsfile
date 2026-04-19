pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18'
                }
            }
            steps {
                sh '''
                    export NPM_CONFIG_CACHE=$WORKSPACE/.npm
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    export NPM_CONFIG_CACHE=$WORKSPACE/.npm
                    npm ci
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    export NPM_CONFIG_CACHE=$WORKSPACE/.npm
                    npm ci

                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}