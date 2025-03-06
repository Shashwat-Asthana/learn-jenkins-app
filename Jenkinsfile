// pipeline {
//     agent any

//     stages {
//         stage('Hello') {
//             steps {
//                 echo 'Hello World'
//                 sh 'echo "Hello from Jenkins!"'
//                 sh 'whoami'
//             }
//         }
//     }
// }
// pipeline {
//     agent any

//     stages {
//         stage('Build') {
//             agent{
//                 docker{
//                     image 'node:18-alpine'
//                     reuseNode true
//                 }
//             }
//             steps {
//                 sh '''
//                     ls -la
//                     node --version
//                     npm --version
//                     npm ci
//                     npm run build
//                     ls -la
//                 '''
//             }
//         }
//     }
// }
pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '59ba7004-44f3-4af8-8a17-c232805400e0'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
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
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh 'npm install netlify-cli'
                sh 'node_modules/.bin/netlify --version'
                sh 'echo "Deploying to production. Site ID : $NETLIFY_SITE_ID"'
                sh 'node_modules/.bin/netlify status'
                // Additional deployment steps..
            }
        }

    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
