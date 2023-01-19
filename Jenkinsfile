node{
    echo "The Job Name is: ${env.JOB_NAME}" 
    echo "The Build number is: ${env.BUILD_NUMBER}" 
    echo "The Node is name is: ${env.NODE_NAME}" 
    echo "The Jenkins HD is: ${env.JENKINS_HOME}"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: 'maven3.8.7'
    stage('CheckOutCode'){
        git branch: 'development', credentialsId: '3420acb6-43db-4449-9f20-ebf2f73bb9a4', url: 'https://github.com/pramod-techpark/maven-web-application.git'
    }
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
  /*
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
    */
}
