pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '54.166.202.146'  // Replace with your EC2 instance IP
        SSH_CREDENTIALS = 'ec2-ssh-key'  // Jenkins SSH credential ID
        APP_DIR = '/var/www/node-app'  // Directory on EC2 where the app is deployed
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                echo 'Fetching code from GitHub...'
                git branch: 'main', url: 'https://github.com/Adnan-khan-360/node-app.git', credentialsId: 'jenkins'
            }
        }

        stage('Install Dependencies Locally') {
            steps {
                echo 'Installing dependencies locally...'
                sh 'npm install'
            }
        }

        stage('Deploy to EC2 and Install Dependencies') {
            steps {
                echo 'Deploying code to EC2...'
                sshagent(credentials: ["${SSH_CREDENTIALS}"]) {
                    sh """
                    echo 'Creating target directory if it does not exist...'
                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} "mkdir -p ${APP_DIR}"

                    echo 'Transferring code to EC2...'
                    rsync -avz --exclude 'node_modules' ./ ${EC2_USER}@${EC2_HOST}:${APP_DIR}/

                    echo 'Running npm install on EC2...'
                    ssh ${EC2_USER}@${EC2_HOST} "cd ${APP_DIR} && pwd && ls -la && npm install || { echo 'npm install failed'; exit 1; }"
                    """
                }
            }
        }
    }
}
