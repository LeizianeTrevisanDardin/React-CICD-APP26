pipeline {
    agent none

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:22.14.0-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html 
                    npm test
                '''
            }
        }
    }
}