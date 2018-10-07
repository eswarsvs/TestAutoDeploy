#!groovy
import groovy.json.JsonSlurperClassic
node {
def BUILD_NUMBER=env.BUILD_NUMBER
def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
def DEPLOY_FOLDER = "mdapi_output_dir_${BUILD_NUMBER}"
def SFDC_USERNAME
def HUB_ORG=env.HUB_ORG_DH
def SFDC_HOST = env.SFDC_HOST_DH
def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
def toolbelt = tool 'toolbelt'
stage('Deploying to Sandbox') {
sh "mkdir ${DEPLOY_FOLDER}"
  sh "xcopy ${DEPLOY_FOLDER} c:\users\eswararao.sobila\Eswar\salesforce_ant_39.0\sample\codepkg /e"  
}
}
