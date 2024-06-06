pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ttl.sh/hesamzkr-python-app"
        STAGING_SSH_KEY = 'staging-key'
        PRODUCTION_SSH_KEY = 'staging-key'
        ANSIBLE_HOST_KEY_CHECKING = 'false'
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
        stage('Deploy to Staging') {
            steps {
                ansiblePlaybook credentialsId: STAGING_SSH_KEY,
                                inventory: 'staging-hosts.ini',
                                playbook: 'deploy-staging.yml'
            }
        }
        stage('Deploy to Production') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'staging-key', keyFileVariable: 'SSH_KEY_FILE')]) {
                        sh 'echo "$SSH_KEY_FILE" > /tmp/staging-key && chmod 600 /tmp/staging-key' 

                        sh """
                            ssh -o StrictHostKeyChecking=no -i /tmp/staging-key ubuntu@ec2-16-170-224-192.eu-north-1.compute.amazonaws.com <<EOF
                            docker stop hesamzkr-python-app || true
                            docker rm hesamzkr-python-app || true
                            docker run -d -p 4444:4444 --name hesamzkr-python-app ttl.sh/hesamzkr-python-app:latest
                            EOF
                        """

                        sh 'rm /tmp/staging-key'
                    }
                }
            }
        }
    }
}

