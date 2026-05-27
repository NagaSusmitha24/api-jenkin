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

                bat 'mvn deploy -DmuleDeploy -DmuleVersion=4.4.0 -Dusername=c60aada09ed34f2598dae48f26082b78 -Dpassword=Eccc1Ddf6d02467495879aCA4f75D2DB -DworkerType=MICRO -Dworkers=1 -Dregion=us-west-2' 

            } 

        } 

    } 

} 
