pipeline {
    agent any 
    stages {
        stage('code') {
            steps {
                 git branch: 'main',
                    url: 'https://github.com/xvis/docker-course-three-tier-web-app.git'
            }
        }
        stage('build') {
            steps {
                echo 'building code ..'
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
