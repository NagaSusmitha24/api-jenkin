pipeline {
    agent any

    stages {

        stage('Build & Deploy') {

            steps {

                withCredentials([
                    string(credentialsId: 'mule-client-id', variable: 'CLIENT_ID'),
                    string(credentialsId: 'mule-client-secret', variable: 'CLIENT_SECRET')
                ]) {

                    sh '''
                    mvn clean deploy \
                    -DskipTests \
                    -DmuleDeploy \
                    -Dconnected.app.client.id=c60aada09ed34f2598dae48f26082b78 \
                    -Dconnected.app.client.secret=Eccc1Ddf6d02467495879aCA4f75D2DB
                    '''
                }
            }
        }
    }
}
