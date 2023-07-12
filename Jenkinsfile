pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the GitHub repository
                git url:"https://github.com/prajwalpd7/django-notes-app.git", branch: "main"
            }
        }
        stage('Build') {
            steps {
                // Build Docker image and tag it with a version
                sh "docker build -t notes_app ."
                }
        }
        stage('Push to Docker Hub') {
            steps {
                // pushing the image to docker hub
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes_app ${env.dockerHubUser}/notes_app:latest"        
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes_app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the application to AWS Elastic Beanstalk
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
    
