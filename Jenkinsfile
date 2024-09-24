pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapp'
        K8S_DEPLOYMENT = 'myapp-deployment'
        DOCKER_REGISTRY = 'mydockerregistry.com' // Your Docker registry URL
        DOCKER_IMAGE = "${DOCKER_REGISTRY}/${IMAGE_NAME}:latest"
        SLACK_WEBHOOK_URL = credentials('slack-webhook') // Slack webhook credential
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE ./dockerfiles'
                    // Login to Docker registry
                    sh 'echo $DOCKER_PASSWORD | docker login $DOCKER_REGISTRY -u $DOCKER_USERNAME --password-stdin'
                    // Push the Docker image
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        
        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        script {
                            // Run unit tests
                            sh 'npm install'
                            sh 'npm test'
                        }
                    }
                }
                stage('Integration Tests') {
                    steps {
                        script {
                            // Run integration tests
                            sh 'npm run integration-test'
                        }
                    }
                }
            }
        }
        
        stage('Deploy to Development') {
            steps {
                script {
                    // Apply Kubernetes manifests for development
                    sh 'kubectl apply -f microservices/k8s-manifests/deployments/myapp-deployment-dev.yaml'
                    sh 'kubectl apply -f microservices/k8s-manifests/services/myapp-service-dev.yaml'
                }
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                script {
                    // Apply Kubernetes manifests for production
                    sh 'kubectl apply -f microservices/k8s-manifests/deployments/myapp-deployment-prod.yaml'
                    sh 'kubectl apply -f microservices/k8s-manifests/services/myapp-service-prod.yaml'
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                script {
                    // Cleanup old Docker images
                    sh 'docker rmi $(docker images -f "dangling=true" -q) || true'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
            // Send success notification to Slack
            sh "curl -X POST -H 'Content-type: application/json' --data '{\"text\":\"Deployment to Kubernetes was successful!\"}' $SLACK_WEBHOOK_URL"
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
            // Send failure notification to Slack
            sh "curl -X POST -H 'Content-type: application/json' --data '{\"text\":\"Deployment failed. Please check the logs.\"}' $SLACK_WEBHOOK_URL"
        }
    }
}
