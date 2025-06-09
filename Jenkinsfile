pipeline {
  agent any
  stages {
    stage('Clone') {
      steps { git 'https://github.com/youruser/yourrepo.git' }
    }
    stage('Build') {
      steps { sh 'npm install' }
    }
    stage('Test') {
      steps { sh 'npm test || true' } // Add tests as needed
    }
    stage('Docker Build & Run') {
      steps {
        script {
          docker.build("sample-app:${env.BUILD_NUMBER}")
          sh 'docker run -d -p 3000:3000 sample-app:${env.BUILD_NUMBER}'
        }
      }
    }
  }
  post {
    success {
      mail to: 'team@example.com',
           subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "The build was successful!"
    }
    failure {
      mail to: 'team@example.com',
           subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "The build failed. Check Jenkins for details."
    }
  }
}
