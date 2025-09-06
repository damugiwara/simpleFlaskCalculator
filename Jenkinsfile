pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'fakepride/simple_flask_calculator'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/damugiwara/simpleFlaskCalculator', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        echo "Logging in to Docker Hub..."
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
        success {
            echo "Docker image built and pushed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
