pipeline {
  agent any

  environment {
    S3_BUCKET = "tglobal-tglobl-apple "
    AWS_REGION = "us-east-1"
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/Kubernaik/Frontend-App/edit/master/Jenkinsfile#L6C26'
      }
    }

    stage('Build') {
      steps {
        sh '''
          cd frontend
          npm install
          npm run build
        '''
      }
    }

    stage('Upload to S3') {
      steps {
        sh '''
          aws s3 sync frontend/build s3://$S3_BUCKET --delete
        '''
      }
    }

    stage('Invalidate CloudFront') {
      steps {
        sh '''
          aws cloudfront create-invalidation \
          --distribution-id E123456789 \
          --paths "/*"
        '''
      }
    }
  }
}
pipeline {
  agent any

  stages {
    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Build') {
      steps {
        sh 'echo "Static frontend build completed"'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy to S3 + CloudFront'
      }
    }
  }
}
