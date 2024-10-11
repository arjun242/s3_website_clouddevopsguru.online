pipeline {
    agent any

    environment {
        S3_BUCKET = 'profile.clouddevopsguru.online'       // Your S3 bucket name
        S3_PATH = 'index.html'                  // The path to index.html in S3
        LOCAL_INDEX_FILE = 'index.html'         // Path to index.html in the cloned repo (adjust if it's in a folder)
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
                    """

                    // Upload all .jpg, .jpeg, and .png files
                    sh """
                    aws s3 cp . s3://$S3_BUCKET/ --exclude "*" --include "*.jpg" --include "*.jpeg" --include "*.png" --recursive
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
