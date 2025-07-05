pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
                sh '''
                echo "without docker"
                ls -la
                '''
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    label '' // optional
                }
            }
            steps {
                sh '''
                ls -la
                npm --version
                node --version                
                npm ci
                npm run build
                ls -la
                '''
            }
        }
    }
}
