pipeline {
    agent{
        docker{
            image 'mcr.microsoft.com/playwright:v1.55.0-noble'
            reuseNode true
        }
    }

    environment{
        NETLIFY_SITE_ID = '78b4a1d1-544d-42e7-9658-932781a919c8'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('build') {
            steps {
                sh '''
                    #ls -al
                    #node --version
                    #npm --version
                    #npm ci
                    #npm run build
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
                    #npm install serve
                    #npx serve -s build & sleep 10
                    #npx playwright install
                    #npx playwright test
                '''
            }
        }
        
        stage('Deploy'){
            steps{
                sh '''
                    #npm install netlify-cli@latest
                    #node_modules/.bin/netlify --version
                    #node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
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
