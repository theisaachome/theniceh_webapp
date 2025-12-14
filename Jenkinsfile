pipeline {
    agent any

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
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            steps {
                 sh '''
                   # Verify that the build directory exists and contains index.html
                   if [ -f dist/index.html ]; then
                       echo "Build directory and index.html found."
                   else
                       echo "Build directory or index.html not found!"
                       exit 1
                   fi

                   # Run unit tests
                   npm test
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
     post {
        always {
            junit '**/test-results/*.xml' 
        }
    }
}