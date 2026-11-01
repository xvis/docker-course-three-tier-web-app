
pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
    }

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

        // 6. Docker Hub Login
        stage('Docker Hub Login') {
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
                        printf '%s' "$DOCKERHUB_TOKEN" | docker login \
                            -u "$DOCKERHUB_USER" --password-stdin
                    '''
                }
            }
        }

        // 7. Tag and Push all images
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

                        docker tag api:latest "$DOCKERHUB_USER/three-tier-api:latest"
                        docker tag app:latest "$DOCKERHUB_USER/three-tier-app:latest"
                        docker tag db:latest "$DOCKERHUB_USER/three-tier-db:latest"

                        docker push "$DOCKERHUB_USER/three-tier-api:latest"
                        docker push "$DOCKERHUB_USER/three-tier-app:latest"
                        docker push "$DOCKERHUB_USER/three-tier-db:latest"

                        docker logout
                    '''
                }
            }
        }

        // 8. Verify images
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
