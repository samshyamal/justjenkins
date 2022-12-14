#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME
    def alias="shyamal"
    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
    def TEST_LEVEL = 'RunLocalTests'

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY

    def toolbelt = tool 'toolbelt'




    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }


    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // ----
      withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('authorisatiom') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }else{
                 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
            if (rc != 0) { error 'hub org authorization failed' }


        }
        stage('Create Test Scratch Org') {
            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:org:create --targetdevhubusername avinesh17@force.com --setdefaultusername --definitionfile config/project-scratch-def.json --setalias ${alias} --wait 10 --durationdays 1"
              }
 
        stage('set password for org') {
            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:user:password:generate --targetusername ${alias}"
        }         

        stage('Display scratch org stats') {
            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:user:display --targetusername ${alias}"
        }



         stage('deploy scratch org stats') {
            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:mdapi:deploy -d metadataPackage_1670931275320 -u ${alias} -w 5"
        }

            
 


        
    }
   
}
