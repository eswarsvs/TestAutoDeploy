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
stage('checkout source') {
// when running in multi-branch job, one must issue this command
checkout scm
}
withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
stage('Create Scratch Org') {
//rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}" 
rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile C:/Users/eswararao.sobila/Eswar/Salesforce/SalesforceDX/certificates/server.key --setdefaultdevhubusername --instanceurl ${SFDC_HOST}" 

  if (rc != 0) { error 'hub org authorization failed' }
// need to pull out assigned username
rmsg = sh returnStdout: true, script: "${toolbelt}/sfdx  force:org:create --definitionfile config/project-scratch-def.json --json --setdefaultusername"
//rmsg = sh returnStdout: true, script: "${toolbelt}/sfdx force:org:create -s -f config/project-scratch-def.json -a TestScratch"  
printf rmsg
println(rmsg)  
println('Hello From Job HDL')  
def jsonSlurper = new JsonSlurperClassic()
def robj = jsonSlurper.parseText(rmsg)
//if (robj.status != "ok") { error 'org creation failed: ' + robj.message }
SFDC_USERNAME=robj.result.username
robj = null
}
stage('Push To Test Org') {
rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:source:push --targetusername ${SFDC_USERNAME}"
if (rc != 0) {
error 'push failed'
}
// assign permset
//rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:user:permset:assign --targetusername ${SFDC_USERNAME} --permsetname DreamHouse"
//if (rc != 0) {
//error 'permset:assign failed'
//}
}
//stage('Run Apex Test') {
//sh "mkdir -p ${RUN_ARTIFACT_DIR}"
//timeout(time: 120, unit: 'SECONDS') {
//rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:apex:test:run --testlevel RunLocalTests --outputdir ${RUN_ARTIFACT_DIR} --resultformat tap --targetusername ${SFDC_USERNAME} --wait 3"
//if (rc != 0) {
//error 'apex test run failed'
//}
//}
//}
//stage('collect results') {
//junit keepLongStdio: true, testResults: 'tests/**/*-junit.xml'
//}
stage('Deploying to Sandbox') {
sh "mkdir ${DEPLOY_FOLDER}"
rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:source:convert -d ${DEPLOY_FOLDER} --packagename package_name"
if (rc != 0) 
  {
   error 'Error while preparing package'
}
rc = sh returnStatus: true, script: "${toolbelt}/sfdx  force:mdapi:deploy -d ${DEPLOY_FOLDER} -u ${HUB_ORG} --wait 2"
if(rc != 0){
  error 'Error while Deploying'    
  }
}
stage('copying ant files') {
  sh "xcopy C:\Users\eswararao.sobila/Eswar/Salesforce/Jenkins/workspace/SFDXAutoDeploy_Simple-L526GJMYX4ONG3UVT2YMGKVZLNDG3GTYENJXYJULJCDDWJOV6APQ/mdapi_output_dir_13 C:/Users/eswararao.sobila/Eswar/salesforce_ant_39.0/sample/codepkg /e"  
    sh "cd C:/Users/eswararao.sobila/Eswar/salesforce_ant_39.0/sample"
}
}
}
