pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="277130520464.dkr.ecr.us-east-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'myapp'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws configure set aws_access_key_id AKIAUBBSEE6IKDAENLOK
         aws configure set aws_secret_access_key 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
         aws configure set default.region us-east-1
         #echo $AWS_ACCESS_KEY_ID
         #DOCKER_LOGIN_PASS=$(aws ecr get-login-password  --region us-east-1)
         #docker login -u AWS -p $DOCKER_LOGIN_PASS https://678828690512.dkr.ecr.us-east-1.amazonaws.com/helloworld
         aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 277130520464.dkr.ecr.us-east-1.amazonaws.com
	 docker build -t 277130520464.dkr.ecr.us-east-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 277130520464.dkr.ecr.us-east-1.amazonaws.com/helloworld:SAMPLE-PROJECT-${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
            sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
	    mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
            kubectl apply -f deployment-new.yaml -n $K8S_NAMESPACE
            kubectl apply -f service.yaml -n $K8S_NAMESPACE
           '''
      }
    }
  }
}
