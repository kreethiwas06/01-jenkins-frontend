pipeline {
    agent any
    
    environment{
        S3_BUCKET=         frontend-demo-proj
        DISTRIBUTION_ID=   E1Y8SQ50DEXOL8
    }
    stages {
        stage('gitclone'){
            steps {
                git branch: 'master'
                git credentialsId: 'keypem',
                url: 'https://github.com/kreethiwas06/sample-devops-frontend-project.git'
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
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'access key secret key', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh'''
                    /usr/local/bin/aws s3 sync out/s3://${S3_BUCKET} --delete
                    '''
                }
            }
        }
        stage('cloudfront create-invalidation'){
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'access key secret key', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
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
