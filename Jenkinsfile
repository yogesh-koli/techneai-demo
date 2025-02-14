pipeline {
    agent any
    environment {
        IMAGE_TAG = "yogik001/nginx-demo:${BUILD_NUMBER}"  // Use unique build number for versioning
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/yogesh-koli/techneai-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh '''
                    chmod +x *.sh || true # Ensure scripts are executable if needed
                    docker build -t $IMAGE_TAG .
                '''
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: 'https://index.docker.io/v1/']) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'docker push $IMAGE_TAG'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    if kubectl get deployment nginx-demo; then
                        kubectl set image deployment/nginx-demo nginx=$IMAGE_TAG --record
                        kubectl rollout restart deployment/nginx-demo
                    else
                        kubectl apply -f k8s-deployment.yml
                    fi
                '''
            }
        }
    }
}

