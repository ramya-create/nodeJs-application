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
        stage('Check Kubernetes Cluster') {
          steps {
            bat 'kubectl config current-context'
            bat 'kubectl cluster-info'
            bat 'kubectl get nodes'
          }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def deleteStatus = bat(script: 'kubectl delete -f deployment.yaml', returnStatus: true)
                    if (deleteStatus != 0) {
                        echo 'No existing deployment to delete or deletion failed, continuing...'
                    }
                }
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
                script {  // Get Minikube IP
            def minikubeIp = bat(script: 'minikube ip', returnStdout: true).trim()
            
            // Get service JSON and save to a file to avoid parsing issues
            bat 'kubectl get svc my-node-service -o json > svc.json'
            
            // Read JSON from the saved file
            def json = readJSON file: 'svc.json'
            
            // Extract nodePort from JSON
            def nodePort = json.spec.ports[0].nodePort
            
            // Compose app URL
            def appUrl = "http://${minikubeIp}:${nodePort}"
            
            echo "🌐 Application URL: ${appUrl}"
                }
            }
        }
    }
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
