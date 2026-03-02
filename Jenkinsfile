pipeline {
    agent any

    environment {
        SSH_CREDENTIALS = 'wordpress-app-key'   // Jenkins SSH credential ID
        SERVER_IP = '172.31.19.16'
        REMOTE_USER = 'ubuntu'
        DEPLOY_DIR = '/var/www/html'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/yoginigavali035-ux/Fitlife-Gym-jenkins-Devops.git'
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(credentials: [SSH_CREDENTIALS]) {
                    sh """
                        scp -o StrictHostKeyChecking=no -r * ${REMOTE_USER}@${SERVER_IP}:${DEPLOY_DIR}
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${SERVER_IP} 'sudo systemctl restart nginx'
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Website Deployed Successfully!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}