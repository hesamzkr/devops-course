pipeline {
    agent any

    stages {
        stage('Build image') {
            steps {
                sh 'docker build -t ttl.sh/hesamzkr-python-app .'
                sh 'docker images'
            }
        }
        stage('Push image') {
            steps {
                sh 'docker push ttl.sh/hesamzkr-python-app'
            }
        }
        stage('Deploy to target') {
            environment {
                ANSIBLE_HOST_KEY_CHECKING = 'false'
            }
            steps {
                ansiblePlaybook credentialsId: 'mykey',
                                inventory: 'hosts.ini',
                                playbook: 'playbook.yml'
            }
        }
    }
}
