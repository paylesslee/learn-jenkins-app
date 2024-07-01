pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
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
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                    ls -la
                '''
            }
        }
        
    }

    post {
        always {
            sh 'checkov -f tfplan.json --framework terraform_plan --soft-fail -o cli -o junitxml --output-file-path results.xml'
            junit skipPublishingChecks: true, testResults: '**/*.xml'
        }
    }
}
