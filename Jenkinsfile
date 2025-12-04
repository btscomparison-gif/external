pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-cred')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t flaskapp:v1 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo 'Logging in to Docker Hub'
                sh 'echo "$DOCKER_HUB_CREDENTIALS_PSW" | docker login -u "$DOCKER_HUB_CREDENTIALS_USR" --password-stdin'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                echo 'Pushing image to Docker Hub'
                sh '''
                    docker tag flaskapp:v1 52106/flaskapp1:kubeimage1
                    docker push 52106/flaskapp1:kubeimage1
   
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes'
                sh 'kubectl apply -f deployment.yaml --validate=false'
                sh 'kubectl apply -f service.yaml'
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
