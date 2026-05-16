pipeline {
    agent any

    environment {
        WEBLOGIC_JAR = '/home/vinay/Oracle/Middleware/Oracle_Home/wlserver/server/lib/weblogic.jar'
        ADMIN_URL    = 't3://192.168.32.128:7001'
        APP_NAME     = 'benefits'
        TARGETS      = 'JVM1' 
        
        // FIX: Just use the filename. Jenkins clones this from GitHub into its active workspace automatically.
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
                withCredentials([usernamePassword(credentialsId: 'weblogic-admin-creds', 
                                                 usernameVariable: 'WLS_USER', 
                                                 passwordVariable: 'WLS_PASS')]) {
                    
                    // FIX: Changing to triple single-quotes (''') stops Groovy from breaking the <(bash) syntax
                    sh '''
                        #!/bin/bash
                        
                        # 1. Source the environment via Bash process substitution safely
                        source <(bash /home/vinay/Oracle/Middleware/Oracle_Home/user_projects/domains/base_domain/bin/setDomainEnv.sh)
                        
                        # 2. Execute WebLogic deployer natively using the loaded environment classpaths
                        java weblogic.Deployer \
                        -adminurl "${ADMIN_URL}" \
                        -username "${WLS_USER}" \
                        -password "${WLS_PASS}" \
                        -deploy \
                        -name "${APP_NAME}" \
                        -source "${WAR_FILE}" \
                        -targets "${TARGETS}" \
                        -verbose
                    '''
                }
            }
        }
    }
}

