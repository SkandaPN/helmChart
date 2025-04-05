pipeline {
    agent any

    environment {
        registry = "public.ecr.aws/i9s0c4w6/ecrrepo"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SkandaPN/helmChart.git'
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
                    sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/i9s0c4w6"
                    sh "docker push public.ecr.aws/i9s0c4w6/ecrrepo:latest"
                    
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
