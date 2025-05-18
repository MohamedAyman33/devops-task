pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDS = credentials('dockerhub')
        APP_IMAGE = "m7mdayman/devops-flask-app:${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Test Docker') {
            steps {
                sh 'docker --version'
                sh 'docker ps'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${APP_IMAGE} ."
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                sh "echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin"
                sh "docker push ${APP_IMAGE}"
            }
        }
        
        stage('Deploy') {
            steps {
                sh """
                docker stop flask-app || true
                docker rm flask-app || true
                docker run -d --name flask-app -p 5000:5000 ${APP_IMAGE}
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
