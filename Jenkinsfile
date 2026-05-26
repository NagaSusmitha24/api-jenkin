pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Build & Package') {
            steps {
                echo 'Building Mule Application...'
                bat '''
                mvn clean package ^
                -DskipTests ^
                -s C:\\Users\\Admin\\.m2\\settings.xml
                '''
            }
        }

        stage('Publish to Exchange') {
            steps {
                echo 'Publishing Mule Application to Anypoint Exchange...'
                bat '''
                mvn deploy ^
                -DskipTests ^
                -Danypoint.username=kancharlanaga ^
                -Danypoint.password=Susmitha@123 ^
                -s C:\\Users\\Admin\\.m2\\settings.xml
                '''
            }
        }

        stage('Deploy to CloudHub 2.0') {
            steps {
                echo 'Deploying Mule Application to CloudHub 2.0...'
                bat '''
                mvn mule:deploy ^
                -DmuleDeploy ^
                -DskipTests ^
                -Danypoint.username=kancharlanaga ^
                -Danypoint.password=Susmitha@123 ^
                -s C:\\Users\\Admin\\.m2\\settings.xml
                '''
            }
        }
    }

    post {
        success {
            echo 'Mule Application Published to Exchange and Deployed to CloudHub 2.0 Successfully!'
        }
        failure {
            echo 'Pipeline Failed. Check Jenkins logs.'
        }
    }
}
