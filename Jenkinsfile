pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')  // Jenkins credential ID
        DOCKER_IMAGE = "rahulks01/react-jenkins-docker-k8s"  // Docker Hub image name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rahulks01/react-jenkins-docker-k8s.git'  // Git repository URL
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)  // Build the Docker image
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Authenticate to Docker Hub and push the built image
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image(DOCKER_IMAGE).push("latest")  // Push the image with "latest" tag
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes deployment and service YAML files
                    sh "kubectl apply -f ~/k8s-deployment/k8s-deployment.yaml"
                    sh "kubectl apply -f ~/k8s-deployment/k8s-service.yaml"
                }
            }
        }
    }
}
