pipeline {
    agent any
    environment {
        IMAGE_TAG = "yogik001/nginx-demo:latest"
    }

    stages {
        stage('Setup Permissions') {
            steps {
                sh 'chmod -R 755 $WORKSPACE' // Ensure proper execution rights
            }
        }
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/yogesh-koli/techneai-demo.git'
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
                withDockerRegistry([credentialsId: '001', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push $IMAGE_TAG'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl set image deployment/nginx-demo nginx=$IMAGE_TAG --record || echo "Failed to update image"
                    kubectl apply -f k8s-deployment.yml || echo "Failed to apply Kubernetes manifest"
                '''
            }
        }
    }
}
