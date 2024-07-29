pipeline {
    agents any
    stages {
        stage ('build') {
            steps {
                sh '''
                whoami
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 590183962065.dkr.ecr.us-east-1.amazonaws.com
                docker build -t node_project .
                docker tag node_project:latest 590183962065.dkr.ecr.us-east-1.amazonaws.com/node_project:${BUILD_NUMBER}
                docker push 590183962065.dkr.ecr.us-east-1.amazonaws.com/node_project:${BUILD_NUMBER}
                '''
            }

        }
        stage ('deploy'){
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f K8/service.yaml
                '''
            }

        }
    }
}