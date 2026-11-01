pipeline {
    agent any 
    stages {
        stage('code') {
            steps {
                 git branch: 'main',
                    url: 'https://github.com/xvis/docker-course-three-tier-web-app.git'
            }
        }
        stage('check Docker version') {
            steps {
                echo 'docker version'
            }
        }
        stage('build API image'){
            steps{
                sh '''
                docker build -t api:latest ./api
                '''
            }
        }
        stage('build frontend image'){
            steps{
                sh '''
                docker build -t app:latest ./app
                '''
            }
        }
        stage('build database images'){
            steps{
                sh '''
                docker build -t db:latest ./db
                '''
            }
        }
        stage('test') {
            steps{
                echo 'code test ho gaya'
            }
        }
        stage('Deployed'){
            steps{
                sh '''
                docker login

                docker tag api:latest xvishu/three-tier-api:latest
                docker tag app:latest xvishu/three-tier-app:latest
                docker tag db:latest xvishu/three-tier-db:latest

                docker push xvishu/three-tier-api:latest
                docker push xvishu/three-tier-app:latest
                docker push xvishu/three-tier-db:latest
                '''
            }
        }
    }
}
