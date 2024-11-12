pipeline {
    agent any
    
    parameters {
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Enable deployment to Docker Hub?')
        string(name: 'DOCKER_TAG', defaultValue: 'latest', description: 'Docker tag for production image')
    }
    
    environment {
        DOCKER_HUB_USERNAME = 'moeedasif38'
        IMAGE_NAME = "${DOCKER_HUB_USERNAME}/calculator-app"
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies'
                // Add your actual dependency installation commands here
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                // Add your actual test commands here
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build image with specified tag
                    docker.build("${IMAGE_NAME}:${params.DOCKER_TAG}")
                    echo "Docker image built with tag ${params.DOCKER_TAG}"
                }
            }
        }
        
        stage('Push Docker Image to Docker Hub') {
            when {
                expression { return params.DEPLOY }
            }
            steps {
                script {
                    // Use Docker Hub credentials to login
                    withCredentials([usernamePassword(
                        credentialsId: "${DOCKER_CREDENTIALS}", 
                        usernameVariable: 'DOCKER_USER', 
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh """
                            echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                            docker push ${IMAGE_NAME}:${params.DOCKER_TAG}
                            docker logout
                        """
                    }
                    echo "Docker image pushed to Docker Hub with tag ${params.DOCKER_TAG}"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}