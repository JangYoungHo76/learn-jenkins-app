pipeline {
    agent any

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
    }
}
