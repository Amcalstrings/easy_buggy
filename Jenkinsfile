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

        // building docker image
        stage("build docker image"){
            steps{
                withDockerRegistry([
                    credentialsId: 'dockerlogin', url: ""
                ]){
                    script{
                        app = docker.build("javaapp")
                        
                    }
                }
            }
        }
        // push image to ECR hub
        stage("push to ECR"){
            steps{
                script{
                    docker.withRegistry("https://717279705656.dkr.ecr.us-east-1.amazonaws.com", "ecr:us-east-1:aws-credentials")
                        {
                            app.push("latest")
                        }
                }
            }
     }    

        // Deploy to Kubernetes cluster
        stage("DeployToKubernetes"){
            steps{
                withKubeConfig([credentialsId: "kubelogin"]){
                    sh 'kubectl delete --all -n devsecops'
                    sh 'kubectl apply -f deployment.yaml -n devsecops'
                }
            }
        }
}

}