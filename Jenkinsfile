pipeline {
    agent any

    tools {
        maven 'Maven3'    // Must match Jenkins Global Tool name
        jdk 'JDK8'        // Must match Jenkins Global Tool name
    }

    // ── Change these to match your setup ──────────────────────
    environment {
        ANYPOINT_CREDENTIALS  = credentials('anypoint-credentials')
        APP_NAME              = 'my-mule-app'
        CLOUDHUB_ENV          = 'Sandbox'
        CLOUDHUB_TARGET       = 'Cloudhub-US-East-1'
        MULE_VERSION          = '4.4.0'
        REPLICAS              = '1'
        VCORES                = '0.1'
        BUSINESS_GROUP_ID     = ''   // Leave blank if no business group
    }
    // ──────────────────────────────────────────────────────────

    stages {

        stage('Checkout') {
            steps {
                echo '========== Checking out source code =========='
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '========== Building Mule Application =========='
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo '========== Running Unit Tests =========='
                sh 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy to CloudHub 2.0') {
            steps {
                echo '========== Deploying to CloudHub 2.0 =========='
                sh """
                    mvn deploy -DmuleDeploy \
                        -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} \
                        -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} \
                        -Dapp.name=${APP_NAME} \
                        -Denv=${CLOUDHUB_ENV} \
                        -Dtarget=${CLOUDHUB_TARGET} \
                        -DmuleVersion=${MULE_VERSION} \
                        -Dreplicas=${REPLICAS} \
                        -DvCores=${VCORES} \
                        -DskipTests
                """
            }
        }
    }

    post {
        success {
            echo '=========================================='
            echo " Deployment to CloudHub 2.0 SUCCESSFUL!"
            echo " App: ${APP_NAME}"
            echo " Env: ${CLOUDHUB_ENV}"
            echo '=========================================='
        }
        failure {
            echo '=========================================='
            echo " Deployment FAILED!"
            echo ' Check console output for details.'
            echo '=========================================='
        }
        always {
            cleanWs()  // Clean workspace after build
        }
    }
}
