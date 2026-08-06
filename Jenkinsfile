pipeline {
    agent{
        docker{
            image 'mcr.microsoft.com/playwright:v1.55.0-noble'
            reuseNode true
        }
    }

    stages {
        stage('build') {
            steps {
                sh '''
                    ls -al
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -al
                '''
            }
        }

        stage('Test'){
            steps{
                echo 'Test Stage'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }            
        }

        stage('End To End'){
            steps{
                sh '''
                    npm install serve
                    npx serve -s build & sleep 10
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
