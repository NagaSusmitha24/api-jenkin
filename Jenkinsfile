pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                echo 'Application is in Building Phase'

                bat 'mvn clean install'

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

                CONNECTED_APP_CLIENT_ID = credentials('mule-client-id')
                CONNECTED_APP_CLIENT_SECRET = credentials('mule-client-secret')

            }

            steps {

                bat '''
                mvn deploy ^
                -DmuleDeploy ^
                -DmuleVersion=4.4.0 ^
                -DconnectedAppClientId=%CONNECTED_APP_CLIENT_ID% ^
                -DconnectedAppClientSecret=%CONNECTED_APP_CLIENT_SECRET% ^
                -DconnectedAppGrantType=client_credentials ^
                -DbusinessGroup=469e9fdb-66a0-442a-bd41-668b21a64f7c ^
                -Denvironment=Sandbox ^
                -DworkerType=MICRO ^
                -Dworkers=1 ^
                -Dregion=us-west-2
                '''

            }
        }
    }
}
