pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'JDK'
    }

    environment {
        TOMCAT_HOME = "C:\\maven_tomcat\\apache-tomcat-9.0.117-windows-x64\\apache-tomcat-9.0.117\\"
        CATALINA_HOME = "${TOMCAT_HOME}"
        CATALINA_BASE = "${TOMCAT_HOME}"
        S3_BUCKET = 'jenkinsbucket'

        WAR_FILE = "my-app.war"
        AWS_DEFAULT_REGION = 'eu-north-1'
        
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/DesertFox47/my-app.git', branch: 'main'
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn -version'
                bat 'mvn clean package'
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



      
        stage('Download from S3 & Deploy') {
            steps {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws-creds1'
                    ]]){
                    bat """
                    aws s3 cp s3://jenkinsbucket/my-app.war C:\\maven_tomcat\\apache-tomcat-9.0.117-windows-x64\\apache-tomcat-9.0.117\\webapps
                     """
                    }
            }
        }

        stage('Start Tomcat') {
            steps {
                    bat '''
                    set JAVA_HOME=C:\\Program Files\\Java\\jdk-21.0.11
                    set JRE_HOME=%JAVA_HOME%

                    set TOMCAT_HOME=C:\\maven_tomcat\\apache-tomcat-9.0.117-windows-x64\\apache-tomcat-9.0.117
                    set CATALINA_HOME=%TOMCAT_HOME%
                    set CATALINA_BASE=%TOMCAT_HOME%

                    echo JAVA_HOME=%JAVA_HOME%
                    echo TOMCAT_HOME=%TOMCAT_HOME%

                    cd /d %TOMCAT_HOME%\\bin
                    startup.bat
                    '''
            }
        }
    }
}
