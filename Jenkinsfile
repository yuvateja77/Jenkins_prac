pipeline {
    agent any

    stages {
        //This is comment
        stage('Build') {

            agent {
                docker {
                    image 'node:24-alpine'
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

        stage('Test'){
            agent {
                docker {
                    image 'node:24-alpine'
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

        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.56.1-noble'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install -g serve
                    serve -s build & 
                    sleep 10 
                    npx playwright tests
                '''
            }
        }
    }
}
