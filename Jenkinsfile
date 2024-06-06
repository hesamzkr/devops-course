pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ttl.sh/hesamzkr-python-app"
        STAGING_SSH_KEY = 'staging-key'
        PRODUCTION_SSH_KEY = 'production-key'
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
                ansiblePlaybook credentialsId: PRODUCTION_SSH_KEY,
                                inventory: 'production-hosts.ini',
                                playbook: 'deploy-production.yml'
            }
        }
    }
}

