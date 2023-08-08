pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-1'
       AWS_ACCESS_KEY_ID = credentials('my-aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('my-aws-secret-key')
        S3_BUCKET_NAME = 'learningops.info'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "checkout"
            }
        }
        
        stage('Build') {
            steps {
                echo "build"
            }
        }
        
        stage('Deploy to S3') {
            steps {
                script {
                    def awsCliCmd = """
                        aws s3 cp index.html s3://${S3_BUCKET_NAME}/
                    """
                    sh awsCliCmd
                }
            }
        }
    }
}
