pipeline {
    agent any

    tools {
       // Must match Jenkins Global Tool Configuration name
        jdk 'JDK-17'       // Must match Jenkins Global Tool Configuration name
    }

    environment {
        // Jenkins Credentials ID — add in Jenkins → Manage Credentials
        ANYPOINT_CREDENTIALS    = credentials('anypoint-credentials')

        // App & Deployment Config — matches your pom.xml
        APP_NAME                = 'bit-bucket-demo'
        BUSINESS_GROUP_ID       = '0c18259d-596e-4342-88f4-05dc50278018'
        CLOUDHUB_ENV            = 'Sandbox'
        CLOUDHUB_TARGET         = 'Cloudhub-US-East-2'
        MULE_VERSION            = '4.9.17'
        REPLICAS                = '1'
        VCORES                  = '0.1'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '===== Checking out code from repository ====='
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '===== Building Mule Application ====='
                bat 'mvn clean package -DskipTests'
            }
            post {
                success {
                    echo 'Build SUCCESS'
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
                failure {
                    echo 'Build FAILED'
                }
            }
        }

        stage('Test') {
            steps {
                echo '===== Running Tests ====='
                bat 'mvn test'
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
                echo '===== Deploying to CloudHub 2.0 ====='
                bat """
                    mvn deploy -DmuleDeploy \
                        -Danypoint.username=Durga-May \
                        -Danypoint.password=Durga@53 \
                        -DbusinessGroup=0c18259d-596e-4342-88f4-05dc50278018 \
                        -Denv=Sandbox \
                        -Dtarget=Cloudhub-US-East-2 \
                        -DmuleVersion=4.9.17 \
                        -Dreplicas=1 \
                        -DvCores=0.1 \
                        -DskipTests \
                        -s C:\Users\Admin\.m2\settings.xml
                """
            }
            post {
                success {
                    echo """
                    ============================================
                     Deployment to CloudHub 2.0 SUCCESSFUL!
                     App Name : ${APP_NAME}
                     Env      : ${CLOUDHUB_ENV}
                     Target   : ${CLOUDHUB_TARGET}
                    ============================================
                    """
                }
                failure {
                    echo """
                    ============================================
                     Deployment FAILED!
                     Check console output for details.
                    ============================================
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed — check the logs!'
        }
        always {
            cleanWs()
        }
    }
}
