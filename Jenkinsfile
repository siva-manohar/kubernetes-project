pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="713179705108.dkr.ecr.us-east-1.amazonaws.com"
    K8S_NAMESPACE = 'nodejs'
    K8S_DEPLOYMENT_NAME = 'kubeproject'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin $DOCKER_REGISTRY
	 docker build -t nodejs_k8s .
         docker tag nodejs_k8s:latest $DOCKER_REGISTRY/nodejs_k8s:${BUILD_NUMBER}
         docker push $DOCKER_REGISTRY/nodejs_k8s:${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
            sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
	    mkdir -p $HOME/bin && cp kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
            kubectl apply -f deployment-new.yaml -n $K8S_NAMESPACE
            kubectl apply -f K8/service.yaml -n $K8S_NAMESPACE
           '''
      }
    }
  }
}
