pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true                    
        }
    }

    stages {
        stage('Build Stage') {
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

        stage('Test Stage'){
            steps{
                echo "Test Stage"
                sh '''
                    test -f build/index.html
                    npm test
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
