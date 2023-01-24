node{
    echo "The Job Name is: ${env.JOB_NAME}" 
    echo "The Build number is: ${env.BUILD_NUMBER}" 
    echo "The Node is name is: ${env.NODE_NAME}" 
    echo "The Jenkins HD is: ${env.JENKINS_HOME}"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: 'maven3.8.7'
    try{
            stage('CheckOutCode'){
                sendslacknotifications('STARTED')
            git branch: 'development', credentialsId: '3420acb6-43db-4449-9f20-ebf2f73bb9a4', url: 'https://github.com/pramod-techpark/maven-web-application.git'
        }
            stage('Build'){
            sh "${mavenHome}/bin/mvn clean package"
        }
  
            stage('SonarQubeReport'){
            sh "${mavenHome}/bin/mvn clean sonar:sonar"
        }
            stage('UploadArtifactintoNexus'){
            sh "${mavenHome}/bin/mvn clean deploy"
        }
            stage('DeployAppintoTomcat'){sshagent(['b4ca12d4-0b1e-40e2-91d8-2846871f32ef']) 
            {
                sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.91.163:/opt/apache-tomcat-9.0.70/webapps"
        }  
        }
    }
    catch(e){
        currentBuild.result="FAILURE"
    }
    finally{
        sendslacknotifications(currentBuild.result)
    }
  
}

  def sendslacknotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#citibank-team')
}
