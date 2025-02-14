pipeline {
    agent any
    environment {
        IMAGE_TAG = "yogik001/nginx-demo:latest"
    }

      stages {
        stage('Test Shell') {
            steps {
                sh 'echo "Jenkins Shell Execution Test"'
            }
        }
    }
    
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/yogesh-koli/techneai-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_TAG ."
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
                kubectl set image deployment/nginx-demo nginx=$IMAGE_TAG --record
                kubectl apply -f k8s-deployment.yaml
                '''
            }
        }
    }
}

