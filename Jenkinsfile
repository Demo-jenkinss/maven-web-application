node {

def mavenHome= tool name: "maven3.6.3"
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
sshagent(['544e922c-306b-4a79-9087-f501d6886220']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@http://18.188.237.189/:/opt/apache-tomcat-9.0.82/webapps/"
}
stage('SendEmailNotification'){
emailext body: '''Build is over!!
Regards,
Harikrishna.''', subject: 'Build is over', to: 'harikrishnaseethapuram@gmail.com'
}
}
