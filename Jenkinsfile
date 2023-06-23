pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="195394092895.dkr.ecr.eu-west-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'project'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin $DOCKER_REGISTRY
	 docker build -t $DOCKER_REGISTRY/project:${BUILD_NUMBER} .
         docker push $DOCKER_REGISTRY/project:${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
            sed "s/newbuildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
            kubectl apply -f deployment-new.yaml -n $K8S_NAMESPACE
            kubectl apply -f service.yaml -n $K8S_NAMESPACE
           '''
      }
    }
  }
}
