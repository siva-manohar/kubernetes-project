pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="678828690512.dkr.ecr.ap-south-1.amazonaws.com"
    K8S_NAMESPACE = 'default'
    K8S_DEPLOYMENT_NAME = 'myapp'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
         aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
         aws configure set default.region ap-south-1
         echo $AWS_ACCESS_KEY_ID
         DOCKER_LOGIN_PASS=$(aws ecr get-login-password  --region ap-south-1)
         docker login -u AWS -p $DOCKER_LOGIN_PASS https://678828690512.dkr.ecr.ap-south-1.amazonaws.com/helloworld
         docker build -t 678828690512.dkr.ecr.ap-south-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 678828690512.dkr.ecr.ap-south-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
            sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
            kubectl apply -f deployment-new.yaml -n $K8S_NAMESPACE
            kubectl apply -f service.yaml -n $K8S_NAMESPACE
           '''
      }
    }
  }
}
