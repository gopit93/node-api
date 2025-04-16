pipeline {
    agent any
    environment {
        IMAGE_NAME = 'node-api'
        CONTAINER_NAME = 'node-api-container'
        JENKINS_CONTAINER_NAME = 'jenkins'
    }
    stages {
        stage('Clone Repo') {
            steps {
                git credentialsId: 'github-creds', branch: 'main', url: 'https://github.com/gopit93/node-api.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    COMMIT_HASH = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    TIMESTAMP = sh(script: "date +%Y%m%d-%H%M%S", returnStdout: true).trim()
                    IMAGE_TAG = "${IMAGE_NAME}:${TIMESTAMP}-${COMMIT_HASH}"
                    env.IMAGE_TAG = IMAGE_TAG
                    sh "docker build -t $IMAGE_TAG ."
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                // Ensure we don't stop or remove the Jenkins container
                sh '''
                    if [ "$(docker ps -q -f name=$CONTAINER_NAME)" ]; then
                        docker stop $CONTAINER_NAME || true
                        docker rm $CONTAINER_NAME || true
                    fi
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name $CONTAINER_NAME $IMAGE_TAG'
            }
        }

        stage('Tag as Latest') {
            steps {
                sh 'docker tag $IMAGE_TAG $IMAGE_NAME:latest'
            }
        }
    }
}
