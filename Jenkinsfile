pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hesamzkr-python-app'
        DOCKER_REGISTRY = 'ttl.sh'
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
                    sh 'kubectl --kubeconfig=$KUBECONFIG get componentstatuses'
                    sh 'kubectl --kubeconfig=$KUBECONFIG apply -f k8s-deployment.yml --validate=false'
                }
            }
       }
    }
}

