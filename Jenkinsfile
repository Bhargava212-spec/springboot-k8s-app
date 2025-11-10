pipeline {
    agent any

    environment {
        IMAGE_NAME = "springboot-app"
        IMAGE_TAG = "v1"
        DEPLOYMENT_FILE = "deployment.yaml"
        KUBECONFIG = "/root/.kube/config"
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    branch: 'master',
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


        stage('Fix kubeconfig paths') {
            steps {
                sh '''
                    # Replace Windows paths with Linux paths in kubeconfig
                    sed -i 's|C:\\\\Users\\\\BhargavaMakkena|/root|g' ${KUBECONFIG}
                '''
            }
        }


        stage('Validate Minikube Setup') {
            steps {
                sh '''
                    echo "Validating Minikube certs and kubeconfig..."
                    if [ ! -f /root/.minikube/profiles/minikube/client.crt ] || \
                       [ ! -f /root/.minikube/profiles/minikube/client.key ] || \
                       [ ! -f /root/.minikube/ca.crt ]; then
                        echo "ERROR: Minikube certs missing! Please mount ~/.minikube and ~/.kube into Jenkins container."
                        exit 1
                    fi
                    echo "Validation successful: Minikube certs found."
                '''
            }
        }



        stage('Deploy to Minikube') {
            steps {
                sh "kubectl apply -f ${DEPLOYMENT_FILE}"
            }
        }
    }
}