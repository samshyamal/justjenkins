#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY

    def sfdx = tool 'sfdx'




    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }


    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------
    
    withEnv(["HOME=${env.WORKSPACE}"]) {
        
        withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {

            // -------------------------------------------------------------------------
            // Authorize the Dev Hub org with JWT key and give it an alias.
            // -------------------------------------------------------------------------

            stage('Authorize DevHub') {
               rc = bat returnStatus: true, script: "${sfdx} sfdx force:auth:jwt:grant --clientid 3MVG9n_HvETGhr3AY.ORKMs45bSz6521MMTJPoHhtlF2Sr6NzOGMNdi7Nw4FUzalrNNo.5.FIsxyS1v1GmfQg --jwtkeyfile  \"${jwt_key_file}\" --username dasari.avinesh-vt6x@force.com --instanceurl https://login.salesforce.com --setdefaultdevhubusername"
                if (rc != 0) {
                    error 'Salesforce dev hub org authorization failed.'
                }
            }
        }    
    }
}
