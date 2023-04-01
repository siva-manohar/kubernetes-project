pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="599032366287.dkr.ecr.ap-south-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'project'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws configure set aws_access_key_id AKIAYW6I4QTH5TJ7G5VF
         aws configure set aws_secret_access_key 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
         aws configure set default.region ap-south-1
         #echo $AWS_ACCESS_KEY_ID
         #DOCKER_LOGIN_PASS=$(aws ecr get-login-password  --region us-east-1)
         #docker login -u AWS -p $DOCKER_LOGIN_PASS https://678828690512.dkr.ecr.us-east-1.amazonaws.com/helloworld
         aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 599032366287.dkr.ecr.ap-south-1.amazonaws.com
	 docker build -t 599032366287.dkr.ecr.ap-south-1.amazonaws.com/project1:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 599032366287.dkr.ecr.ap-south-1.amazonaws.com/project1:SAMPLE-PROJECT-${BUILD_NUMBER}
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
