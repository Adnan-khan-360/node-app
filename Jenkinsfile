pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18' // Ensure NodeJS is configured in Jenkins
    }

    environment {
        NODE_ENV = 'production'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning old workspace...'
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                echo 'Fetching code from GitHub...'
                git branch: 'main', url: 'https://github.com/Adnan-khan-360/node-app.git', credentialsId: 'jenkins'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Stopping the Node.js service...'
                sh 'sudo systemctl stop node-data.service || true'

                echo 'Starting the Node.js service...'
                sh 'sudo systemctl daemon-reload'
                sh 'sudo systemctl start node-data.service'
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully!'
            sh 'sudo systemctl status node-data.service'
        }
        failure {
            echo 'Build failed. Check the logs for details.'
            sh 'sudo systemctl status node-data.service'
        }
    }
}

