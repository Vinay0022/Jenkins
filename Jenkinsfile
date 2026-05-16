pipeline {
    agent any

    environment {
        WEBLOGIC_JAR = '/home/vinay/Jenkins/benefits.war' // <-- Change this
        ADMIN_URL    = 't3://192.168.32.128:7001'
        APP_NAME     = 'my-web-app'
        TARGETS      = 'JVM1' // <-- Change this if needed
        WAR_FILE     = 'benefits.war'  // <-- Change to your exact file name
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to WebLogic') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'weblogic-admin-creds', 
                                                 usernameVariable: 'WLS_USER', 
                                                 passwordVariable: 'WLS_PASS')]) {
                    sh """
                        java -cp ${WEBLOGIC_JAR} weblogic.Deployer \
                        -adminurl ${ADMIN_URL} \
                        -username ${WLS_USER} \
                        -password ${WLS_PASS} \
                        -redeploy \
                        -name ${APP_NAME} \
                        -source ${WAR_FILE} \
                        -targets ${TARGETS} \
                        -verbose
                    """
                }
            }
        }
    }
}

