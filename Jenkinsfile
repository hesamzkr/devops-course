pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hesamzkr-python-app'
        DOCKER_REGISTRY = 'ttl.sh'
        DEPLOYMENT_FILE = 'k8s-deployment.yml'
        SSH_CREDENTIALS = 'k8s-credentials'
        // SSH_TARGET = 'jenkins@192.168.105.4'
    }

    stages {
        stage('Build image') {
            steps {
                sh 'docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE} .'
            }
        }
        stage('Push image') {
            steps {
                sh 'docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'k8s-credentials', variable: 'KUBECONFIG')]) {
                    sh 'cat ${KUBECONFIG}' // Debugging step to view the KUBECONFIG content
                    sh 'kubectl --kubeconfig=$KUBECONFIG apply -f ${DEPLOYMENT_FILE}'
                }
            }
        }
    }
}

