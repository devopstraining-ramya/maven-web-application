node{
def mavenHome = tool name: "maven 3.9.6" 
echo "jenkins url is: ${env.JENKINS_URL}"
echo "node name is: ${env.NODE_NAME}"
echo "job name is: ${env.JOB_NAME}"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckOutCode'){
git branch: 'development', credentialsId: 'ab6f26ef-4873-47c4-84b6-a68f343a25d0', url: 'https://github.com/devopstraining-ramya/maven-web-application.git'
}
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExcuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage('UploadArtifactIntoArtifactRepository'){
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('DeployAppIntoTomcatServer'){
sshagent(['d401d579-afd0-4044-956f-28016615601a']) {
sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.0.244:/opt/apache-tomcat-9.0.86/webapps/"

}
}
}
