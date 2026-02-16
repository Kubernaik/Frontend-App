pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'devop-project-realtime12345'
        CLOUDFRONT_DISTRIBUTION_ID = 'E1B2ER65WAV8HE'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Kubernaik/Frontend-App.git'
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                  chmod +x build.sh
                  ./build.sh
                '''
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'AWS-Credentails'
                ]]) {
                    sh '''
                      aws s3 sync build/ s3://$S3_BUCKET --delete
                      aws cloudfront create-invalidation \
                        --distribution-id $CLOUDFRONT_DISTRIBUTION_ID \
                        --paths "/*"
                    '''
                }
            }
        }
    }
}
