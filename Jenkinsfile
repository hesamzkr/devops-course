pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ttl.sh/hesamzkr-python-app'
        SSH_KEY_ID = 'staging-key'
    }

    stages {
        stage('Build image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }
        stage('Push image') {
            steps {
                sh 'docker push ${DOCKER_IMAGE}'
            }
        }
        stage('Deploy to Kubernetes') { 
            steps {
                script {
                    sshagent(credentials: [SSH_KEY_ID]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no vagrant@192.168.105.4 <<EOF
                            kubectl --kubeconfig=~/.kube/config apply -f k8s-deployment.yml
                        """
                    }
                }
                // withCredentials([file(credentialsId: 'k8s-credentials', variable: 'KUBECONFIG')]) {
                //     sh 'kubectl --kubeconfig=$KUBECONFIG apply -f k8s-deployment.yml --validate=false'
                // }
            }
       }
    }
}

