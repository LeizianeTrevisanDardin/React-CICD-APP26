pipeline {
    agent any

//CI stage -> CODE -> Build -> Test
    stages {
        stage('Build') {

            agent{
            //before steps we create a docker image using this code below. This info comes from docker hub, find in the terminal which node -v is installed and then in tags -> look for the version and add alpine at the end to find a version that is not heavy.
                docker {
                    image 'node:24.13.0-alpine'
                    reuseNode true
                }
            }
            steps {
                //this is step is to run in the image what we would in the terminal - the ls -la is to check what is in the folder
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm install
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test'){
            agent{
               docker{
                image 'node:24.13.0-alpine'
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
}