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
            mvn mule:deploy -DskipTests ^
            -Danypoint.username=kancharlanaga ^
            -Danypoint.password=Susmitha@123
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
