pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    environment {
        DOCKER_IMAGE = 'my-node-app'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/ramya-create/nodeJs-application.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t my-node-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker rm -f node-app || true'
                bat 'docker run -d --name node-app -p 3000:3000 my-node-app'
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace and releasing agent...'
            cleanWs()
        }
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
