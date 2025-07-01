// MODIFIED CODE
pipeline {
    agent {
        docker {
            image 'node:24-alpine'
            reuseNode true
        }
    }

    environment {
        NETLIFY_SITE_ID = 'e6a13e1e-1573-4e3a-af26-5cae2ef0feda'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Prepare Environment') {
            steps {
                sh '''
                    apk add --no-cache bash
                    node --version
                    npm --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    npm ci
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    npm run build
                    ls -la build
                '''
            }
        }

        stage('Deploy to Netlify') {
            steps {
                sh '''
                    npm install -g netlify-cli
                    netlify --version
                    echo "Deploying to Production with SITE ID: $NETLIFY_SITE_ID"
                    netlify status
                    netlify deploy --dir=build --prod --site=$NETLIFY_SITE_ID
                '''
            }
        }
    }
}






// OLD CODE
// pipeline {
//     agent any

//     environment {
//         NETLIFY_SITE_ID = 'e6a13e1e-1573-4e3a-af26-5cae2ef0feda'
//         NETLIFY_AUTH_TOKEN = credentials('netlify-token')
//     }

//     stages {

//         stage('Build') {
//             agent {
//                 docker {
//                     image 'node:24-alpine'
//                     reuseNode true
//                 }
//             }
//             steps {
//                 sh '''
//                     ls -la
//                     node --version
//                     npm --version
//                     npm ci
//                     npm run build
//                     ls -la
//                 '''
//             }
//         }

//         stage('Deploy') {
//                     agent {
//                         docker {
//                             image 'node:24-alpine'
//                             reuseNode true
//                         }
//                     }
//                     steps {
//                         sh '''
//                             npm install netlify-cli
//                             node_modules/.bin/netlify --version
//                             echo 'Deploying to Production with SITE ID: $NETLIFY_SITE_ID'
//                             node_modules/.bin/netlify status
//                             node_modules/.bin/netlify deploy --dir=build --prod
//                         '''
//                     }
//         }
//     }
// }
