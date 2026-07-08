pipeline {
    agent any
    
    environment{
        S3_BUCKET=         "kiaq-07"
        DISTRIBUTION_ID=   "E13WLMS0BOD6PP"
    }
    stages {
        stage('gitclone'){
            steps {git branch: 'main', credentialsId: 'kreethiwas06', url: 'https://github.com/kreethiwas06/01-jenkins-frontend.git'
            }
        }
        stage('install dependencies'){
            steps {
                sh 'npm install'
            }
        }
        stage('build static files'){
            steps {
                sh 'run npm build'
            }
        }
        stage('s3 bucket'){
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'kiaq_07', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')])  {
                    sh'''
                    /usr/local/bin/aws s3 sync out/s3://${S3_BUCKET} --delete
                    '''
                }
            }
        }
        stage('cloudfront create-invalidation'){
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'kiaq_07', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh'''
                    /usr/local/bin/aws cloufront create-invalidation \
                    --disribution-id${'DISTRIBUTION_ID}
                    --paths "/*"
                    '''
                }
            }
        }
    }
}
