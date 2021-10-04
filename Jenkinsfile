node {
def mavenHome = tool name: "maven 3.6.2"
  stage ('CheckoutCode'){
  git branch: 'development', credentialsId: '035be35e-fbf2-405f-8d33-d820cb78804c', 
  url: 'https://github.com/harishgowdabr/maven-web-application.git'
  
  }// stage Git
	stage('Build'){
	sh "${mavenHome}/bin/mvn clean package"
	
	} // stage Build
	
	
	stage('ExexuteSonarQubeReport'){
	sh "${mavenHome}/bin/mvn clean sonar:sonar"
	
	} // stage sonar
	
	stage('UploadArtifactIntoNexusRepo'){
	sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
	
	} // stage nexus
	
	stage('DeploytheAppIntoTomcat'){
	 sshagent(['bfe5536c-540c-44f6-9b9f-522e563ebf96']){
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@54.146.244.68:/opt/apache-tomcat-9.0.53/webapps"
	}//ssh
	} //stage  tomcat
	
	stage('SendEmailNotification'){
	mail bcc: 'harishkumarbr8@gmail.com', body: '''Build Over.

    Thanks,
      ASG''', cc: 'harishkumarbr8@gmail.com', from: '', replyTo: '', subject: 'BuuildOver!!!', to: 'harishkumarbr8@gmail.com'
    }


	}// node
