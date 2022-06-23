node{
    
def mavenHome = tool name: "maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode'){
git branch: 'development', credentialsId: 'e1b7d1e1-e590-40ad-8b06-57ff8cd55d1c', url: 'https://github.com/nsktechnologies/maven-web-application.git'
}

stage('Build'){
  sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"    
}

stage('DeployAppIntoTomcatServer'){
 sshagent(['02cc2258-1ce8-4cb9-8df6-83770d0b3d2d']) {
  sh "scp -o StrictHostKeyChecking=NO target/maven-web-application.war ec2-user@54.163.44.138:/opt/apache-tomcat-9.0.63/webapps"
}
}

stage('SendEmailNotificaton'){
emailext body: 'Build is over!!', subject: 'Build is over!!', to: 'sarathmca15@gmail.com'

}
}



