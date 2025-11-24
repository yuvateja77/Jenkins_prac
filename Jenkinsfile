pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'a2bfe9db-9982-45c0-8335-7417222cfcf3'
        NETLIFY_AUTH_TOKEN = credentials('nelify-token')
    }

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

        stage('Deploy') {

            agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            
            steps {
                sh '''
                    npm install netlify-cli -g
                    netlify -v 
                    echo "deploying to production site id : $NETLIFY_SITE_ID"
                    netlify status
                    netlify deploy --prod --dir=build --skip-build
                '''
            }
        }
    }
}
