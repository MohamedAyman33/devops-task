pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Info') {
            steps {
                sh 'whoami'
                sh 'ls -la /var/run/docker.sock || echo "Docker socket not accessible"'
                sh 'id'
            }
        }
        
        stage('Deploy Simple Flask') {
            steps {
                sh '''
                echo "Creating simple Flask app deployment"
                echo "This would normally involve Docker but we'll simulate it for now"
                echo "Hello from DevOps!" > index.html
                '''
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
