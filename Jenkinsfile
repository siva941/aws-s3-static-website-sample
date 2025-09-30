pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = 'AKIAQEFWATJ2RQPCXY7J'      // 🔑 Replace with your Access Key
        AWS_SECRET_ACCESS_KEY = 'IdOSu9QGvBGejY/V1fleTVkc/ZQlh2H+T52hvCn3'      // 🔑 Replace with your Secret Key
        AWS_DEFAULT_REGION    = 'us-east-1'
        S3_BUCKET             = 'devopsfrontend2369'       // ✅ Replace with actual bucket
        CLOUDFRONT_ID         = 'E1HV4188SK738H'           // ✅ Replace with your CloudFront distribution ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/siva941/aws-s3-static-website-sample.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'  // Faster & safer than npm install
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    echo "Syncing build directory to S3..."
                    aws s3 sync build/ s3://$S3_BUCKET --delete --exact-timestamps
                '''
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                sh '''
                    echo "Invalidating CloudFront cache..."
                    aws cloudfront create-invalidation \
                        --distribution-id $CLOUDFRONT_ID \
                        --paths "/*"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed. Check logs."
        }
    }
}




