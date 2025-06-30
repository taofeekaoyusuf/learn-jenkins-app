pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'e6a13e1e-1573-4e3a-af26-5cae2ef0feda'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
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
                    test -f src/index.js
                    test -f src/index.css
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
                            npm install netlify-cli
                            node_modules/.bin/netlify --version
                            echo 'Deploying to Production with SITE ID: $NETLIFY_SITE_ID'
                            node_modules/.bin/netlify status
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