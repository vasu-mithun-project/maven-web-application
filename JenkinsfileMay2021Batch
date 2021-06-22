node
{
def mavenHome = tool name: "maven-3.8.1" 
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
stage('CheckoutCode')
{          
git branch: 'development', credentialsId: '0885eeb1-a271-41a3-a750-8149ccbcd919', url: 'https://github.com/vasu-mithun-project/maven-web-application.git'
}
stage('Build')
{
 sh "${mavenHome}/bin/mvn clean package" 	        
}
stage('ExecuteSonarReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
 stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployAppIntoTomcatServer')
{
sshagent(['8cc6ab41-2951-4680-89c4-aa577e96c94b']) 
{
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.197.124.43:/opt/apache-tomcat-9.0.46/webapps/"
}
}
stage('SendEmail Notification')
{
emailext body: '''Build is over!!
Regards,
MithunTechnologies.''', subject: 'Build is over', to: 'vasu.jenkinsmails@gmail.com'
}
    
}
