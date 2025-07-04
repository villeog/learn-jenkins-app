pipeline {
    agent any

    stages {
        stage('Without Docker') {
            steps {
                sh '''
                echo "w/o docker"
                touch wodo2
                ls -la
                '''
            }
        }

        stage('With Docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    label '' // optional
                }
            }
            steps {
                sh 'echo "with docker"'
                sh '''
                npm --version
                touch withd2
                ls -la
                '''
            }
        }
    }
}

