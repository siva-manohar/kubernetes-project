pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="092101872227.dkr.ecr.ap-southeast-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'kubeproject'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin $DOCKER_REGISTRY
	 docker build -t automation-docker .
         docker tag automation-docker:latest $DOCKER_REGISTRY/automation-docker:${BUILD_NUMBER}
         docker push $DOCKER_REGISTRY/automation-docker:${BUILD_NUMBER}
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
