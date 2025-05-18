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
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${APP_IMAGE} ."
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', 
                                 passwordVariable: 'DOCKER_PASSWORD', 
                                 usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker push ${APP_IMAGE}"
                }
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
