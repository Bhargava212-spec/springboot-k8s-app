pipeline {
    agent any

    environment {
        IMAGE_NAME = "springboot-app"
        IMAGE_TAG = "v1"
        DEPLOYMENT_FILE = "deployment.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/Bhargava212-spec/springboot-k8s-app',
                    credentialsId: 'github-creds'
                )
            }
        }

        stage('Set Minikube Docker Env') {
            steps {
                sh "eval \$(minikube docker-env)"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh "kubectl apply -f ${DEPLOYMENT_FILE}"
            }
        }
    }
}