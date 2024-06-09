pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ttl.sh/hesamzkr-python-app'
        SSH_KEY_ID = 'k8s-ssh-key'
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
			    scp -o StrictHostKeychecking=no k8s-deployment.yml vagrant@192.168.56.4
			    ssh -o StrictHostKeyChecking=no vagrant@192.168.105.4 <<EOF
                            kubectl apply -f k8s-deployment.yml
                        """
                    } 
                }
           }
       }
    }
}

