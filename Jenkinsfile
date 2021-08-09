node()
{
 def mavenHome = tool name : "maven3.8.1"    
 properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
 stage('checkoutcode')
 {
  git branch: 'development', credentialsId: 'ebfbffdb-1b98-46dc-847d-3b6d95c3e519', url: 'https://github.com/jithusan-ecc-apps/maven-web-application.git'     
 }
 stage('Build')
 {
 sh "${mavenHome}/bin/mvn clean package"     
 }
stage("ExecuteSonarQubeReport")
 {
    sh "${mavenHome}/bin/mvn sonar:sonar" 
 }

  stage("uploadArtifacts") 
  {
  sh "${mavenHome}/bin/mvn deploy"      
  }
 stage("deployAppintoTomcat")
 {
 sshagent(['7ad2e166-c925-4bc1-a6a8-07c8f3595189'])
 {
 sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.106.245:/opt/tomcat/webapps"     
 }
 }
stage('sendEmailNotification')
{
mail bcc: '', body: '''Buildover
Regargs,
jithendra''', cc: 'jithendrasathish@gmail.com', from: '', replyTo: '', subject: 'Buildover', to: 'yjnsvt.sateesh@gmail.com'
}
}
