pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "my-ecommerce-backend"
        DOCKER_TAG = "${BUILD_NUMBER}"
        REGISTRY = "juhichoudhary/my-ecommerce"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch : 'main', url: 'https://github.com/Juhi5863/ecommerce_Project_Jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} backend/"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withDockerRegistry([credentialsId: '6a280542-7619-4c87-9db0-a1208d2b1bc5', url: '']) {
                        sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${REGISTRY}:${DOCKER_TAG}"
                        sh "docker push ${REGISTRY}:${DOCKER_TAG}"
                    }
                }
            }
        }

        stage('Deploy to Local Docker') {
            steps {
                script {
                    sh """
                    docker stop ecommerce || true
                    docker rm ecommerce || true
                    docker run -d --name ecommerce -p 5000:5000 ${REGISTRY}:${DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
