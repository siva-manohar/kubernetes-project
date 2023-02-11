pipeline {
  agent {
    kubernetes {
      label 'mypod'
    }
  }

  environment {
    DOCKER_REGISTRY = ''
    IMAGE_NAME = ''
    IMAGE_TAG = env.BRANCH_NAME.toLowerCase() + '-' + env.BUILD_NUMBER
    K8S_NAMESPACE = 'default'
    K8S_DEPLOYMENT_NAME = 'myapp'
    DOCKERFILE_LOCATION = './Dockerfile'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -f $DOCKERFILE_LOCATION -t $DOCKER_REGISTRY$IMAGE_NAME:$IMAGE_TAG .'
      }
    }

    stage('Push Docker Image') {
      steps {
        sh 'docker push $DOCKER_REGISTRY$IMAGE_NAME:$IMAGE_TAG'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh "kubectl set image deployment/$K8S_DEPLOYMENT_NAME $IMAGE_NAME=$DOCKER_REGISTRY$IMAGE_NAME:$IMAGE_TAG -n $K8S_NAMESPACE"
      }
    }
  }
}
