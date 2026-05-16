pipeline {
    agent any

    environment {
        WEBLOGIC_JAR = '/home/vinay/Oracle/Middleware/Oracle_Home/wlserver/server/lib/weblogic.jar'
        ADMIN_URL    = 't3://192.168.32.128:7001'
        APP_NAME     = 'benefits'
        TARGETS      = 'JVM1'
        WAR_FILE     = 'benefits.war'
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to WebLogic') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'weblogic-admin-creds',
                        usernameVariable: 'WLS_USER',
                        passwordVariable: 'WLS_PASS'
                    )
                ]) {

                    sh '''
                        #!/bin/bash

                        echo "Current Workspace:"
                        pwd

                        echo "Files in Workspace:"
                        ls -ltr

                        # Source WebLogic Environment
                        source /home/vinay/Oracle/Middleware/Oracle_Home/user_projects/domains/base_domain/bin/setDomainEnv.sh

                        # Deploy WAR
                        java -cp "${WEBLOGIC_JAR}" weblogic.Deployer \
                        -adminurl "${ADMIN_URL}" \
                        -username "${WLS_USER}" \
                        -password "${WLS_PASS}" \
                        -deploy \
                        -name "${APP_NAME}" \
                        -source "${WORKSPACE}/${WAR_FILE}" \
                        -targets "${TARGETS}" \
                        -verbose
                    '''
                }
            }
        }
    }
}
