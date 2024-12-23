pipeline{
    agent any 
    tools {
        maven 'maven'
    }
    stages {
        stage('CompileandRunSonarAnalysis'){
            steps{
                withCredentials([string(credentialsId: 'tech366token', variable: 'tech366token')]) {
                    sh 'mvn clean verify sonar:sonar -Dsonar.login=$tech366token -Dsonar.organization=tech366 -Dsonar.host.url=https://sonarcloud.io -Dsonar.projectKey=tech366' 
                }
            }
        }
        // stages()
     }    
}