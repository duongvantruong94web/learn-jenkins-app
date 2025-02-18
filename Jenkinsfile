pipeline {
    agent any

    stages {
        //This a comment
        /*
        multiple comments
        */
        stage('Build') {
            agent{
                docker{
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
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                //commnet inside the sh using: #
                sh '''
                    echo "Test stage"
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('E2E'){
            agent{
                docker{
                    image 'docker pull mcr.microsoft.com/playwright:v1.50.1-noble'
                    reuseNode true
                }
            }
            steps{
                //commnet inside the sh using: #
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    npx playwright test
                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }

}