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
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }
    }
}