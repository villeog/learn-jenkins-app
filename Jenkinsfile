pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '18d2fe31-7c73-48ec-b4d0-fc12c774f3f6'
        NETLIFY_AUTH_TOKEN = credentials('netlify_token')
        
    }

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
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    label '' // optional
                }
            }
            steps{
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }      

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    label '' // optional
                }
            }
            steps {
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod                
                echo "Deploying to produciton. Site ID: $NETLIFY_SITE_ID"

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
