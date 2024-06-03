pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hesamzkr-python-app'
        DOCKER_REGISTRY = 'ttl.sh'
        DEPLOYMENT_FILE = 'k8s-deployment.yml'
        CREDENTIALS_ID = 'kubeconfig-id'
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
                withKubeConfig([credentialsId: '${CREDENTIALS_ID}']) {
                    sh 'kubectl apply -f ${DEPLOYMENT_FILE} --validate=false'
                }
           }
        }
    }
}

