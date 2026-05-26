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

        stage('Build') {
            steps {
                echo 'Building Mule Application...'

                bat '''
                mvn clean package ^
                -DskipTests ^
                -s C:\\Users\\Admin\\.m2\\settings.xml
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running MUnit Tests...'

                bat '''
                mvn test ^
                -s C:\\Users\\Admin\\.m2\\settings.xml
                '''
            }
        }

        stage('Deploy') {
            steps {

                echo 'Deploying Mule Application...'

                bat '''
                mvn deploy ^
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
            echo 'Mule Application Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed. Check Jenkins logs.'
        }
    }
}
