pipeline {
    agent any

    environment {//I will get the site id from netlify and paste as in the variable
        NETLIFY_SITE_ID = '28275bfc-0647-424e-ba17-c73e7e3ff056' //this variable needs to be setup on Jenkins as well
        //-> go to Netlify and then profile -> user account -> settings -> authorization -> Oath -> generate a new token ->copy it
        //in Jenkins got to Settings and then Credentials -> system -> Global Credentials
        //then choose secret text -> 

        NETLIFY_AUTH_TOKEN = credentials('myToken1') //use the name of your token variable here as safe to push to github
    }

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

        stage('Deploy') {
            agent{
                docker{
                    image 'node:24.13.0-alpine'
                    reuseNode true
                }
            }

                    steps{
                        sh '''
                            npm install netlify-cli
                            node_modules/.bin/netlify --version
                            echo "Deploying to production. Site ID:$NETLIFY_SITE_ID"
                            node_modules./bin/netlify status
                            node_modules./bin/netlify deploy --prod --dir=build
                        '''

                }
            }
        }
    }
