pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials' // Replace with your Jenkins credentials ID for Docker Hub
        DOCKER_HUB_USERNAME = 'imbivek01' // Replace with your Docker Hub username
        SERVICES = ['result', 'vote', 'worker'] // Replace with your service names
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/imbivek08/voting-app.git', branch: 'main'
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    // Loop through each service to build and push Docker images
                    SERVICES.each { service ->
                        def imageName = "${DOCKER_HUB_USERNAME}/${service}:${env.BUILD_NUMBER}"

                        // Build the Docker image
                        sh """
                        docker build -t ${imageName} ${service}
                        """

                        // Log in to Docker Hub
                        withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh """
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin
                            """
                        }

                        // Push the Docker image to Docker Hub
                        sh """
                        docker push ${imageName}
                        """
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                // Remove Docker images from the build agent to save space
                sh "docker rmi -f $(docker images -q) || true"
            }
        }
    }

    post {
        always {
            // Clean workspace to avoid leftover files
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
