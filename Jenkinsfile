pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t m7mdayman/devops-flask-app:latest ."
            }
        }
        
        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u m7mdayman -p ${DOCKER_PASSWORD}"
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                sh "docker push m7mdayman/devops-flask-app:latest"
            }
        }
        
        stage('Deploy') {
            steps {
                sh """
                docker stop flask-app || true
                docker rm flask-app || true
                docker run -d --name flask-app -p 5000:5000 m7mdayman/devops-flask-app:latest
                """
            }
        }
        
        stage('Cleanup') {
            steps {
                sh "docker logout || true"
                sh "docker system prune -f || true"
            }
        }
    }
    
    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Please check the logs for details."
        }
    }
}
