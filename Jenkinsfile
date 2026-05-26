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
                echo 'Building MuleSoft Application...'
                bat 'mvn clean package -DskipTests -s C:\\Users\\Admin\\.m2\\settings.xml'
            }
        }

        stage('Test') {
            steps {
                echo 'Running MUnit Tests...'
                bat 'mvn test'
            }
        }

        stage('Deploy to CloudHub 2.0') {
            steps {
                echo 'Deploying to CloudHub 2.0...'
                bat """
                    mvn deploy -DskipTests ^
                    -s C:\\Users\\Admin\\.m2\\settings.xml ^
                    -Danypoint.username=kancharlanaga ^
                    -Danypoint.password=Susmitha@123 ^
                    -Dcloudhub2.organizationId=6c5ad96b-67a8-4bc6-8bb1-12443d5764e1 ^
                    -Dcloudhub2.environment=Sandbox ^
                    -Dcloudhub2.applicationName=api-v1 ^
                    -Dcloudhub2.region=us-east-2 ^
                    -Dcloudhub2.replicas=1 ^
                    -Dcloudhub2.vCores=0.1
                """
            }
        }

    }

    post {
        success {
            echo 'Deployment to CloudHub 2.0 Successful!'
        }
        failure {
            echo 'Deployment Failed. Check logs above.'
        }
    }
}
