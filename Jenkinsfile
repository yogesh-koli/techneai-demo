pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'yogik001/nginx-demo'
        IMAGE_TAG = "${DOCKER_IMAGE}:${BUILD_NUMBER}"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/yogesh-koli/techneai-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t yogik001/nginx-demo:latest ."

            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: 'https://hub.docker.com/']) {
                    sh 'docker push $IMAGE_TAG'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl set image deployment/nginx-demo nginx=$IMAGE_TAG --record
                kubectl apply -f k8s-deployment.yaml
                '''
            }
        }
    }
}
