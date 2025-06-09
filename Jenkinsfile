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
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl delete -f deployment.yaml || echo "No existing deployment to delete"'
                bat 'kubectl apply -f deployment.yaml'
            }
        }
        stage('Expose Service') {
            steps {
                bat 'kubectl apply -f service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'kubectl get pods'
                bat 'kubectl get svc'
            }
        }

        stage('Access App') {
            steps {
                    def appUrl = bat(script: 'minikube service my-node-service --url', returnStdout: true).trim()
                    echo "🌐 Application URL: ${appUrl}"
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
