pipeline {
    agent any

    tools {
        nodejs 'Node18'  // Name you configured in Step 4
    }

    environment {
        DOCKER_IMAGE = 'my-node-app'
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/ramya-create/nodeJs-application.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "No tests found."'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-node-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Clean up if already running
                sh 'docker rm -f node-app || true'
                // Run the container
                sh 'docker run -d --name node-app -p 3000:3000 my-node-app'
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
