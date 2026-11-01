
pipeline {
    agent any

    stages {

        // 1. Clone source code from GitHub
        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/xvis/docker-course-three-tier-web-app.git'
            }
        }

        // 2. Check Docker installation
        stage('Docker Validation') {
            steps {
                sh '''
                    docker --version
                    docker ps
                '''
            }
        }

        // 3. Build API image
        stage('Build API Image') {
            steps {
                sh 'docker build -t api:latest ./api'
            }
        }

        // 4. Build APP image
        stage('Build APP Image') {
            steps {
                sh 'docker build -t app:latest ./app'
            }
        }

        // 5. Build DB image
        stage('Build DB Image') {
            steps {
                sh 'docker build -t db:latest ./db'
            }
        }

        // 6. Tag and Push images to Docker Hub
        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_TOKEN'
                    )
                ]) {
                    sh '''
                        set +x
                        trap 'docker logout >/dev/null 2>&1 || true' EXIT

                        echo "Logging into Docker Hub..."

                        printf '%s' "$DOCKERHUB_TOKEN" | docker login \
                            -u "$DOCKERHUB_USER" --password-stdin

                        echo "Tagging images..."

                        docker tag api:latest "$DOCKERHUB_USER/three-tier-api:latest"
                        docker tag app:latest "$DOCKERHUB_USER/three-tier-app:latest"
                        docker tag db:latest "$DOCKERHUB_USER/three-tier-db:latest"

                        echo "Pushing API image..."
                        docker push "$DOCKERHUB_USER/three-tier-api:latest"

                        echo "Pushing APP image..."
                        docker push "$DOCKERHUB_USER/three-tier-app:latest"

                        echo "Pushing DB image..."
                        docker push "$DOCKERHUB_USER/three-tier-db:latest"

                        echo "All images pushed successfully!"
                    '''
                }
            }
        }

        // 7. Verify Docker images
        stage('Verify Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: All images pushed to Docker Hub!'
        }

        failure {
            echo 'FAILED: Please check Jenkins Console Output.'
        }

        always {
            echo 'Jenkins pipeline execution completed.'
        }
    }
}
