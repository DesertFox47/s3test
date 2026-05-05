pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'eu-north-1'
        S3_BUCKET = 'jenkinsbucket'
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvnd clean package'
            }
        }

        stage('Upload to S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds1'
                ]]) {
                    bat """
                    aws s3 cp target/my-app.war s3://%S3_BUCKET%/
                    """
                }
            }
        }
    }
}
