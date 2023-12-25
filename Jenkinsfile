#!groovy

import groovy.json.JsonSlurperClassic

node {
    def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
    def SF_USERNAME=env.SF_USERNAME
    def SF_SERVER_KEY_CREDENTIALS_ID=env.SF_SERVER_KEY_CREDENTIALS_ID
    def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://login.salesforce.com"
    def TEST_LEVEL='RunLocalTests'

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        checkout scm
    }
    withEnv(["HOME=${env.WORKSPACE}"]){
        withCredentials([file(credentialsId: SF_SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]){
            stage('Authorize DevHub') {
                rc = command "${toolbelt}/sf org login jwt --instance-url ${SF_INSTANCE_URL} --client-id ${SF_CONSUMER_KEY} --username ${SF_USERNAME} --jwt-key-file ${server_key_file} --set-default-dev-hub --alias HubOrg"
                if (rc != 0) {
                    error 'Salesforce dev hub org authorization failed.'
                }
            }
        }
    }
}