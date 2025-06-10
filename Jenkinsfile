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
                git branch: 'main', 
                    url: 'https://github.com/ramya-create/nodeJs-application.git'

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
    // for running docker image
    //     stage('Run Docker Container') {
    //         steps {
    //             // Clean up if already running
    //             bat 'docker rm -f node-app || true'
    //             // Run the container
    //             bat 'docker run -d --name node-app -p 3000:3000 my-node-app'
    //         }
    //     }
    // }
    post {
         always {
            echo 'Cleaning workspace and releasing agent...'
            cleanWs()
            // deleteDir()
        }
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
