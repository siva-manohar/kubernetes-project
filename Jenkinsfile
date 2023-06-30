pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="462693274005.dkr.ecr.us-east-2.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'kubeproject'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin $DOCKER_REGISTRY
	 docker build -t docker-server .
         docker tag docker-server:latest $DOCKER_REGISTRY/docker-server:${BUILD_NUMBER}
         docker push $DOCKER_REGISTRY/docker-server:${BUILD_NUMBER}
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
