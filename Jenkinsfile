pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hesamzkr-python-app'
        DOCKER_REGISTRY = 'ttl.sh'
        DEPLOYMENT_FILE = 'k8s-deployment.yml'
        SSH_CREDENTIALS = 'ssh-credentials'
        SSH_TARGET = 'jenkins@192.168.105.4'
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
                sshagent([SSH_CREDENTIALS]) {
                    sh "ssh ${SSH_TARGET} 'kubectl apply -f ${DEPLOYMENT_FILE} --validate=false'"
                }
           }
        }
    }
}

