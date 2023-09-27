pipeline{
    agent any
    parameters {
      choice choices: ['dev', 'test', 'prod'], description: 'Choose the environment to deploy', name: 'envName'
    }
    stages{
        stage("Maven Build"){
            when {
                expression { params.envName == "dev" }
            }
            steps{
               sh "mvn clean package" 
            }
        }
        stage("nexus upload"){
            steps{
                script{
                    def pom=readMavenPom file: 'pom.xml'
                    def version=pom.version
                    def repoName="doctor-onlinr-release"
                    if(version.endsWith("SNAPSHOT")){
                        repoName="doctor-online-snapshot"
                    }
                
                nexusArtifactUploader artifacts: [[artifactId: 'doctor-online', classifier: '', file: 'target/doctor-online.war', type: 'war']], credentialsId: 'nexus3', groupId: 'in.javahome', nexusUrl: '18.217.156.63:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'doctor-online-snapshot', version: '1.1-SNAPSHOT'
                }
            }
        }
        stage("Deploy To Dev"){
            when {
                expression { params.envName == "dev" }
            }
            steps{
                echo params.envName
                echo "Deploy to dev"
            }
        }
        stage("Deploy To Test"){
            when {
                expression { params.envName == "test" }
            }
            steps{
                echo "Deploy to test"
            }
        }
        stage("Deploy To Prod"){
            when {
                expression { params.envName == "prod" }
            }
            steps{
                echo "Deploy to prod"
            }
        }
    }
}
