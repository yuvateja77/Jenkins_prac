pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '86b2774b-bab2-4c4e-8005-cb6a0d7396d2'
        NETLIFY_AUTH_TOKEN = credentials('nelify-token')
    }

    triggers {
        // This ensures CI/CD triggers automatically on GitHub push
        githubPush()
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
                    # netlify deploy --no-build --prod --dir=build 

                '''
            }
        }
    }
}
