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
                script {
                    kubernetesDeploy(
                        configs: 'k8s-deployment.yml',
                        kubeconfigId: 'kubeconfig-id'
                    )
                }
           }
        }
    }
}

