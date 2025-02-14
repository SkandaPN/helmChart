pipeline {
    agent any

    environment {
        registry = "339712957371.dkr.ecr.ap-south-1.amazonaws.com/ecrfull"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/akannan1087/docker-spring-boot']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 339712957371.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "docker push 339712957371.dkr.ecr.ap-south-1.amazonaws.com/ecrfull:latest"
                    
                }
            }
        }  
                
        
        stage ('Helm Deploy') {
          steps {
            script {
                sh "helm upgrade first --install jan-25-chart --namespace helm-deployment --set image.tag=latest"
                }
            }
        }
    }
}
