pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="092101872227.dkr.ecr.ap-southeast-1.amazonaws.com"
    K8S_NAMESPACE = 'nodejs'
    K8S_DEPLOYMENT_NAME = 'kubeproject'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 092101872227.dkr.ecr.ap-southeast-1.amazonaws.com
	 docker build -t kubernetes_automation .
         docker tag kubernetes_automation:latest $DOCKER_REGISTRY/kubernetes_automation:${BUILD_NUMBER}
         docker push $DOCKER_REGISTRY/kubernetes_automation:${BUILD_NUMBER}
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
