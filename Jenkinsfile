pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/imbivek08/voting-app.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building image"
                }
            }
        }
    }

    post {
        always {
            script {
                def imageTag = "my-docker-image:${env.BUILD_NUMBER}"
                sh "docker rmi ${imageTag} || true"
            }
        }
    }
}