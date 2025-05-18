pipeline {
    agent {
        docker {
            image 'docker:dind'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Push') {
            steps {
                sh "docker build -t m7mdayman/devops-flask-app:latest ."
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u m7mdayman -p ${DOCKER_PASSWORD}"
                    sh "docker push m7mdayman/devops-flask-app:latest"
                }
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
    }
    
    post {
        always {
            sh "docker logout || true"
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed. Please check the logs for details."
        }
    }
}
