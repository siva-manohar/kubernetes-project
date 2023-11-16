pipeline {
    agent any
    stages {
        stage("build"){
            steps{
                sh '''
                aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 642611985562.dkr.ecr.us-east-2.amazonaws.com
                docker build -t project_kubernetes .
                docker tag project_kubernetes:latest 642611985562.dkr.ecr.us-east-2.amazonaws.com/project_kubernetes:${BUILD_NUMBER}
                docker push 642611985562.dkr.ecr.us-east-2.amazonaws.com/project_kubernetes:${BUILD_NUMBER}
                '''
            }
        }
        stage("deploy"){
            steps{
                sh '''
                sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
                kubectl apply -f deployment-new.yaml -n nodejs
                kubectl apply -f K8/service.yaml -n nodejs
                '''
            }
        }
    }
}