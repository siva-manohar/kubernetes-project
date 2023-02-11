pipeline {
  agent {
    kubernetes {
      label 'mypod'
    }
  }

  environment {
    AWS_ACCESS_KEY_ID = $AWS_ACCESS_KEY_ID
    AWS_SECRET_ACCESS_KEY = $AWS_SECRET_ACCESS_KEY
    AWS_DEFAULT_REGION = $AWS_DEFAULT_REGION
    DOCKER_REGISTRY = 'public.ecr.aws/e4y1d5a4/'
    IMAGE_NAME = 'helloworld'
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
