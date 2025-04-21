pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t {docker_image_name}:{tag} .'
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    sh 'docker login -u {docker_username} -p {docker_password}'
                    sh 'docker push {docker_image_name}:{tag}'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker run -d -p 3000:3000 {docker_image_name}:{tag}'
                }
            }
        }
    }
}