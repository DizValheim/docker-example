pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Use the docker build command to build the image from the Dockerfile
                    // and tag it with the current build number
                    docker.build("my-python-app:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Run Container') {
            steps {
                script {
                    // Use the built image to run a container.
                    // The 'withRun' block ensures the container is cleaned up after the stage.
                    docker.image("my-python-app:${env.BUILD_NUMBER}").withRun("-p 8000:5000") { c ->
                        // Wait for a few seconds to let the application start
                        sleep 10
                        
                        // Use curl to test that the application is running
                        sh "curl -s http://localhost:8000"
                    }
                }
            }
        }
    }
}
