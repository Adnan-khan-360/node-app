pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18' // Ensure NodeJS is configured in Jenkins
    }

    environment {
        NODE_ENV = 'production'
    }

    stages {
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

        stage('Start Application') {
            steps {
                echo 'Starting the application...'
                sh 'node app.js'
            }
        }
    }

    post {
        success {
            echo 'Application built and started successfully!'
        }
        failure {
            echo 'Build failed. Check the logs for details.'
        }
    }
}

