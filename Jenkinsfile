pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker { image 'node:18' }
            }
            steps {
                sh '''
                    export HOME=$WORKSPACE
                    npm ci --cache $WORKSPACE/.npm
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
                    export HOME=$WORKSPACE
                    npm ci --cache $WORKSPACE/.npm
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
                    export HOME=$WORKSPACE
                    npm ci --cache $WORKSPACE/.npm

                    npx node_modules/.bin/serve -s build &
                    npx wait-on http://localhost:3000

                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'jest-results/junit.xml'
        }
    }
}