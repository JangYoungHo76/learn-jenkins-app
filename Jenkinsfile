pipeline {
    /*
    agent{
        docker{
            image 'mcr.microsoft.com/playwright:v1.62.0-noble'
            reuseNode true
            image 'amazon/aws-cli'
            args "--entrypoint=''"
        }
    }
    */
    environment{
        NETLIFY_SITE_ID = '78b4a1d1-544d-42e7-9658-932781a919c8'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        agent{
            docker{         
                image 'amazon/aws-cli'
                args "--entrypoint=''"
            }
        }        

        stage('AWS') {
            steps {
                sh 'aws --version'
            }
        }
        
        stage('build') {
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.62.0-noble'
                    reuseNode true            
                }
            }
            
            steps {
                sh '''
                    #ls -al
                    #node --version
                    #npm --version
                    #npm ci
                    npm run build
                    #ls -al
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
                    npx playwright install
                    npx playwright test
                '''
            }
        }

        stage('Deploy Staging'){
            steps{
                sh '''
                    npm install netlify-cli@latest
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build
                    echo 'Triger Test'
                '''
            }
        }    
        
        stage('Deploy Prod'){
            steps{
                sh '''
                    npm install netlify-cli@latest
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                    echo 'Triger Test'
                '''
            }
        }    

        stage('Prod E2E'){
            environment{
                CI_ENVIRONMENT_URL = 'https://aquamarine-travesseiro-7b73f4.netlify.app/'
            }
            
            steps{
                sh '''
                    #npx playwright test --reporter=html
                '''
            }
        }
    }    

    post{
        always{
            junit 'jest-results/junit.xml' 
        }
    }
}
