pipeline {
    agent any
    stages {
        stage("build"){
            steps{
                sh '''
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 232679411998.dkr.ecr.us-east-1.amazonaws.com
                docker build -t app_scanner .
                docker tag app_scanner:latest 232679411998.dkr.ecr.us-east-1.amazonaws.com/app_scanner:${BUILD_NUMBER}
                docker push 232679411998.dkr.ecr.us-east-1.amazonaws.com/app_scanner:${BUILD_NUMBER}
                '''
            }
        }
        stage("deploy"){
            steps{
                sh '''
                sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
                kubectl apply -f deployment-new.yaml -n scanner
                kubectl apply -f K8/service.yaml -n scanner
                '''
            }
        }
    }
}
