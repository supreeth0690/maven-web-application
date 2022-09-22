node{
	def mavenhome = tool name:"Maven 3.8.6"
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), 
	[$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
	echo "jenkins url is:${env. JENKINS_URL}"
	echo "node name is:${env. NODE_NAME}"
	echo "job name is:${env. JOB_NAME}"
	stage ('checkoutCode'){
	git credentialsId: '72038918-1a19-46ac-a0a0-28af1157511a', url: 'https://github.com/Pavi-Ajagol/maven-web-application.git'
	}
	stage ('Build'){
	sh "${mavenhome}/bin/mvn clean package"
	}
	stage('ExecuteSonarQubeReport'){
	sh "${mavenhome}/bin/mvn clean sonar:sonar"
	}
	stage ('UploadArtifactRepo'){
	sh "${mavenhome}/bin/mvn clean deploy"
	}
	stage ('DeployappintoTomcat'){
	sshagent(['9abd5029-ddfe-4870-969b-1bd9cb75372f']) {
   sh " scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.238.217:/opt/apache-tomcat-9.0.65/webapps/"
    }
    }
   }
