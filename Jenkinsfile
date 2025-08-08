pipeline {
    agent any

    environment {
        // This tells Jenkins/WSL to use Windows' Docker socket
        DOCKER_HOST = 'tcp://127.0.0.1:2375'
    }

    stages {

        stage('Check Docker') {
            steps {
                sh 'docker --version || echo "Docker not found!"'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build image using Windows Docker daemon
                    sh 'docker build -t my-python-app:${BUILD_NUMBER} .'
                }
            }
        }
        
        stage('Run Container') {
            steps {
                script {
                    // Run container in detached mode on Windows Docker
                    sh 'docker run -d --name test_app_${BUILD_NUMBER} -p 8000:5000 my-python-app:${BUILD_NUMBER}'
                    
                    // Wait for container to start
                    sleep 10
                    
                    // Test app
                    sh 'curl -s http://localhost:8000 || echo "App not reachable!"'
                    
                    // Stop container
                    sh 'docker stop test_app_${BUILD_NUMBER}'
                    sh 'docker rm test_app_${BUILD_NUMBER}'
                }
            }
        }
    }
}
