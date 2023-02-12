pipeline {
  agent any

  environment {
    AWS_ACCESS_KEY_ID = "AWS_ACCESS_KEY_ID"
    AWS_SECRET_ACCESS_KEY = "AWS_SECRET_ACCESS_KEY"
    AWS_DEFAULT_REGION = "AWS_DEFAULT_REGION"
    DOCKER_REGISTRY="937200147656.dkr.ecr.ap-south-1.amazonaws.com"
    K8S_NAMESPACE = 'default'
    K8S_DEPLOYMENT_NAME = 'myapp'
  }

  stages {
    stage('Build Docker Image') {
      steps {
         aws configure set aws_access_key_id $access_key
         aws configure set aws_secret_access_key $secret_key
         aws configure set default.region ap-south-1
         DOCKER_LOGIN_PASSWORD=$(aws ecr get-login-password  --region ap-south-1)
         docker login -u AWS -p $DOCKER_LOGIN_PASSWORD https://937200147656.dkr.ecr.ap-south-1.amazonaws.com
         docker build -t 937200147656.dkr.ecr.ap-south-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 937200147656.dkr.ecr.ap-south-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER}
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh "kubectl set image deployment/$K8S_DEPLOYMENT_NAME $DOCKER_REGISTRY/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER} -n $K8S_NAMESPACE"
      }
    }
  }
}
