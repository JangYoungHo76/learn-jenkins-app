pipeline {
    agent any

    stages {
        stage('npm install stage') {
            agent{
                docker{
                    image 'node:18-alpine''
                }
            }
            
            steps {
                sh '''
                    sh 'npm --version'
                '''
            }
        }
    }
}
