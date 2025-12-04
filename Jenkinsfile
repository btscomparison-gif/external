pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-cred')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                bat 'docker build -t flaskapp:v1 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo 'Logging in to Docker Hub'
                bat 'echo "%DOCKER_HUB_CREDENTIALS_PSW%" | docker login -u "%DOCKER_HUB_CREDENTIALS_USR%" --password-stdin'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                echo 'Pushing image to Docker Hub'
                bat '''
                    docker tag flaskapp:v1 %DOCKER_HUB_CREDENTIALS_USR%/fapp:v1
                    docker push %DOCKER_HUB_CREDENTIALS_USR%/fapp:v1
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes'
                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline executed'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
