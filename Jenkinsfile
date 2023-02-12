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
        sh '''
         aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
         aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
         aws configure set default.region ap-south-1
//          DOCKER_LOGIN_PASSWORD=$(aws ecr get-login-password  --region ap-south-1)
         sudo docker login -u AWS -p $(aws ecr get-login-password  --region ap-south-1) 937200147656.dkr.ecr.ap-south-1.amazonaws.com/helloworld
         sudo docker build -t 937200147656.dkr.ecr.ap-south-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER} .
         sudo docker push 937200147656.dkr.ecr.ap-south-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh "kubectl set image deployment/$K8S_DEPLOYMENT_NAME $DOCKER_REGISTRY/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER} -n $K8S_NAMESPACE"
      }
    }
  }
}
