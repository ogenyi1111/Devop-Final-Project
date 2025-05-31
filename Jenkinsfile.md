pipeline {
    agent any
    
    environment {
        // Define environment variables
        DOCKER_IMAGE = 'your-app-image'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Clean workspace and checkout code
                cleanWs()
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Add your build steps here
                echo 'Building the application...'
                // Example: sh 'mvn clean package' or 'npm install && npm run build'
            }
        }
        
        stage('Test') {
            steps {
                // Add your test steps here
                echo 'Running tests...'
                // Example: sh 'mvn test' or 'npm test'
            }
        }
        
        stage('Code Quality') {
            steps {
                // Add code quality checks
                echo 'Running code quality checks...'
                // Example: sh 'sonar-scanner' or other code quality tools
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build Docker image
                echo 'Building Docker image...'
                // Example: sh 'docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .'
            }
        }
        
        stage('Push to Registry') {
            steps {
                // Push Docker image to registry
                echo 'Pushing Docker image to registry...'
                // Example: sh 'docker push ${DOCKER_IMAGE}:${DOCKER_TAG}'
            }
        }
        
        stage('Deploy') {
            steps {
                // Add deployment steps
                echo 'Deploying application...'
                // Example: sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
    
    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}