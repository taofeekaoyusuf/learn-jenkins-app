pipeline {
    agent any

    stages {
        stage('w/o Docker') {
            steps {
                sh '''
                    echo "Without Docker"
                    ls -la
                    touch container-no.txt
                '''
            }
        }
        stage('w/ Docker') {
            agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "With Docker"
                    ls -la
                    touch container-yes.txt
                '''
            }
        }
    }
}