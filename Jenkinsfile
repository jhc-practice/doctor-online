pipeline{
    agent any
    stages{
        stage("git checkout"){
            steps{
                git branch: 'main', credentialsId: 'genkin-cred', url: 'https://github.com/jhc-practice/doctor-online'
            }
        }
        stage("maven build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("dev deploy"){
            steps{
               sshagent(['genkin-pipeline']) {
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.38.2:/opt/tomcat9/webapps"
                sh "ssh ec2-user@172.31.38.2 /opt/tomcat9/bin/shutdown.sh"
                sh "ssh ec2-user@172.31.38.2 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
