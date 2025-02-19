  pipeline {
      agent any
      tools {
          customTool 'sfdx'
      }
      environment {
          SFDC_HOST = 'https://login.salesforce.com'
          CONNECTED_APP_CONSUMER_KEY = '3MVG9YDQS5WtC11owpBq8Yxh06P9sNoksMrrd1XpUw1e4R3pxJOCChcXqf0EHydjKW3PEKTQAwDENOLEjEcyX'
          JWT_KEY_FILE = credentials('ae521523-2b6c-4b98-8d3e-9fd2572feb41')
          SF_USERNAME = 'jinhaojkh@163.com'
      }
      stages {
          stage('Authenticate') {
              steps {
                  sh 'sfdx auth:jwt:grant --clientid $CONNECTED_APP_CONSUMER_KEY --jwtkeyfile $JWT_KEY_FILE --username $SF_USERNAME --instanceurl $SFDC_HOST --setdefaultdevhubusername'
              }
          }
          stage('Check Code Quality') {
              steps {
                  sh 'sfdx force:source:convert -d mdapi_output_dir'
                  sh 'sfdx force:mdapi:deploy -d mdapi_output_dir -w -1'
              }
          }
          stage('Run Tests') {
              steps {
                  sh 'sfdx force:apex:test:run --resultformat human --codecoverage --wait 10'
              }
          }
      }
  }
