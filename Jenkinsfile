pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="195394092895.dkr.ecr.eu-west-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'project1'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws configure set aws_access_key_id AKIAS27TFH5PYHTNAEXN
         aws configure set aws_secret_access_key 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 
         aws configure set default.region eu-west-1
         echo $AWS_ACCESS_KEY_ID
         #DOCKER_LOGIN_PASS=$(aws ecr get-login-password  --region us-east-1)
         #docker login -u AWS -p $DOCKER_LOGIN_PASS https://678828690512.dkr.ecr.us-east-1.amazonaws.com/helloworld
         aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin $DOCKER_REGISTRY
	 docker build -t $DOCKER_REGISTRY/project-1:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push $DOCKER_REGISTRY/project-1:SAMPLE-PROJECT-${BUILD_NUMBER}
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
