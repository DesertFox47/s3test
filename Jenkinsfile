pipeline {

    agent any
 
    environment {

        AWS_DEFAULT_REGION = 'eu-central-1'

        S3_BUCKET = 'testnallangi1234'

        S3_FILE = 's3://testnallangi1234/test/Screenshot 2024-08-09 070625.png'

        LOCAL_FILE = 'file.zip'

    }
 
    stages {

        stage('Download from S3') {

            steps {

                withCredentials([[

                    $class: 'AmazonWebServicesCredentialsBinding',

                    credentialsId: 'aws-credentials-id'

                ]]) {

                    bat '''

                        aws s3 ls

                        aws s3 cp s3://testnallangi1234/test/sslrenew C:\\Users\\xxnallan

                    '''

                }

            }

        }

    }

}
 
