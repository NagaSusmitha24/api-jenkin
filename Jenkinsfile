pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '📦 Application is in Building Phase'
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Application is in Testing Phase'
                bat 'mvn test'
            }
        }

        stage('Deploy to CloudHub') {
            environment {
                CONNECTED_APP_CLIENT_ID     = credentials('mule-client-id')
                CONNECTED_APP_CLIENT_SECRET = credentials('mule-client-secret')
            }
            steps {
                echo '🚀 Deploying to CloudHub...'
                bat """
                mvn deploy ^
                  -DmuleDeploy ^
                  -DmuleVersion=4.8.0 ^
                  -DconnectedAppClientId=%CONNECTED_APP_CLIENT_ID% ^
                  -DconnectedAppClientSecret=%CONNECTED_APP_CLIENT_SECRET% ^
                  -DconnectedAppGrantType=client_credentials ^
                  -DbusinessGroupId=469e9fdb-66a0-442a-bd41-668b21a64f7c ^
                  -Denvironment=Dev ^
                  -DworkerType=MICRO ^
                  -Dworkers=1 ^
                  -Dregion=us-west-2
                """
            }
        }
    }

    post {
        success {
            echo "✅ Deployment to CloudHub succeeded!"
        }
        failure {
            echo "❌ Deployment failed. Check logs for details."
        }
    }
}
