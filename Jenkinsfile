pipeline{
    agent any 
    tools {
        maven 'maven'
    }
    stages {
        // sonarcloud analysis
        stage('CompileandRunSonarAnalysis'){
            steps{
                withCredentials([string(credentialsId: 'tech366token', variable: 'tech366token')]) {
                    sh 'mvn clean verify sonar:sonar -Dsonar.login=$tech366token -Dsonar.organization=tech366 -Dsonar.host.url=https://sonarcloud.io -Dsonar.projectKey=tech366' 
                }
            }
        }
        // snyk code analysis
        stage('RunSnykCodeAnalysis'){
            steps{
                withCredentials([string(credentialsId:'snyk_token', variable: 'snyk_token')]){
                    sh 'mvn snyk:test -fn'
                }
            }
        }
     }    
}