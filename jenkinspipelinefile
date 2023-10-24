node {

def mavenHome= tool name: "maven-3.6.3"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), 
[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutcode'){
git branch: 'development', credentialsId: 'ece16f4a-2214-441a-a9c8-cdaea07a1f49', 
url: 'https://github.com/Demo-jenkinss/maven-web-application.git'
}

stage('build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecutethesonarQuberepot'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadartifactintoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcatserver')
sshagent(['a4b9d5df-87c0-4eeb-a7f9-a2ff9200bd2a']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.6.40.109:/opt/tomcat-9.0.80/webapps/"
}
stage('SendEmailNotification'){
emailext body: '''Build is over!!
Regards,
Harikrishna.''', subject: 'Build is over', to: 'harikrishnaseethapuram@gmail.com'
}
}
