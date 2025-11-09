pipeline {
    agent any

    environment {
        IMAGE_NAME = "springboot-app"
        IMAGE_TAG = "v1"
        DOCKER_REGISTRY = "localhost:5000" // Minikube registry
        DEPLOYMENT_FILE = "deployment.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Bhargava212-spec/springboot-k8s-app'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Minikube Registry') {
            steps {
                sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh "kubectl apply -f ${DEPLOYMENT_FILE}"
            }
        }
    }
}