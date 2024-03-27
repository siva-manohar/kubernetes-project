pipeline{
    agent any
    stages{
        stage("build"){
            steps{
                sh '''
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 533267090797.dkr.ecr.us-east-1.amazonaws.com
                docker build -t 533267090797.dkr.ecr.us-east-1.amazonaws.com/kubernetes_project:${BUILD_NUMBER} .
                docker push 533267090797.dkr.ecr.us-east-1.amazonaws.com/kubernetes_project:${BUILD_NUMBER}
                '''
            }

        }
        stage("deploy"){
            steps{
                sh '''
                sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
                kubectl apply -f deployment-new.yaml
                kubectl apply -f K8/service.yaml
                '''
            }

        }
    }
}