node
{

def mavenHome = tool name: "Maven3.8.5"

stage('Checkoutcode')
{
git branch: 'development', credentialsId: 'b3c67393-59cc-4d62-b583-4432dab1fa72', url: 'https://github.com/ASGDevopslearning01/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package" 
}

stage('ExecuteSonarQubeReport') 
{
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactIntoNexus') 
{
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployApplicationintoTomcat')
{
sshagent(['a492576d-6ec1-43dc-947f-e56c38f6896b']) 
{
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@34.203.197.128:/opt/apache-tomcat-9.0.62/webapps"
}
}

stage('SendNotifications')
{
emailext body: '''Build is completed

Regards,
Devops''', subject: 'Build over', to: 'shekargopidevopstesting@gmail.com'
}

}
