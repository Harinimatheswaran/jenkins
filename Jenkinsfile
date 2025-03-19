pipeline {
    agent any

    environment {
        IMAGE_NAME = "Harinimatheswaran/myapp"
        REGISTRY = "docker.io"
        DOCKER_CREDENTIALS_ID = "Docker"
        GITHUB_CREDENTIALS_ID = "github"
        APP_DIR = "/home/vboxuser/jenkins"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: GITHUB_CREDENTIALS_ID, url: 'https://github.com/Harinimatheswaran/jenkins', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    // Ensure the image is properly tagged before pushing
                    sh "docker tag ${IMAGE_NAME}:latest ${REGISTRY}/${IMAGE_NAME}:latest"
                    sh "docker push ${REGISTRY}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                script {
                    sh "docker-compose up -d"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
    }
}


	
