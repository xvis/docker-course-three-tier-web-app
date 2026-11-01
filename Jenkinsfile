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
                docker build -t FE:latest ./app
                '''
            }
        }
        stage('build database images'){
            steps{
                ''' docker build -t db:latest ./db
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
                echo 'code deployed ho gaya'
            }
        }
    }
}
