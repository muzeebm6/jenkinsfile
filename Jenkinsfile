pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.6.3/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/muzeebm6/jenkinsfile.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube Analysis') {
            steps{
                withSonarQubeEnv('Sonar-Server-7.8') {
                    sh "mvn sonar:sonar"
                }
            }
        }
		stage('Code deploy') {
            steps{
				sshagent(['tomcat_server']) {
				  sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@3.82.27.101:/home/ec2-user/apache-tomcat-8.5.93/webapps'
				}
            }
        }
    }
}
