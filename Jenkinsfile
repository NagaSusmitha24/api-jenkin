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

       stage('Test') { 

            steps { 

                echo 'Application is in Testing Phase' 

                bat 'mvn test' 

            } 

        } 

        stage('Deploy to Cloudhub') { 

            environment { 

                ANYPOINT_CREDENTIALS = credentials('anypointplatform') 

            } 

            steps { 

                bat 'mvn deploy -DmuleDeploy -DmuleVersion=4.4.0 -Dusername=chaithanya_june20 -Dpassword=Chaithu@516 -DworkerType=MICRO -Dworkers=1 -Dregion=us-west-2' 

            } 

        } 

    } 

} 
