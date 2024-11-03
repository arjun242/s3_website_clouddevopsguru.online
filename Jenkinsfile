pipeline {
    agent any

    environment {
        S3_BUCKET = 'profile.clouddevopsguru.online'       // Your S3 bucket name
        CLOUDFRONT_ID = 'E2YBXRQBAS1AAE'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub (use your repo URL)
                git branch: 'main', url: 'https://github.com/arjun242/s3_website_clouddevopsguru.online.git'
            }
        }

        stage('Upload index.html to S3') {
            steps {
                script {
                    // Upload index.html
                    sh """
                    aws s3 cp index.html s3://$S3_BUCKET/
                    aws s3 cp assets/ s3://$S3_BUCKET/ --recursive
                    aws s3 cp images/ s3://$S3_BUCKETt/ --recursive
                    aws s3 cp download.html s3://$S3_BUCKET/

                    """

                    // Upload all .jpg, .jpeg, and .png files
                    sh """
                    aws s3 cp . s3://$S3_BUCKET/ --exclude "*" --include "*.jpg" --include "*.jpeg" --include "*.PNG" --recursive
                    """
                }
            }
        }
        stage('Invalidate CloudFront Cache') {
            steps {
                script {
                    // Invalidate CloudFront cache
                    sh """
                    aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_ID --paths "/*"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'index.html has been successfully updated in S3!'
        }
        failure {
            echo 'Failed to update index.html in S3.'
        }
    }
}
