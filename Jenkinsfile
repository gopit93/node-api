pipeline {
    agent any

    environment {
        IMAGE_NAME = 'node-api'
        DEV_TAG = 'dev'
        TEST_TAG = 'test'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://your-repo-url.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:$DEV_TAG ."
                }
            }
        }

        stage('Run Dev Container') {
            steps {
                script {
                    sh "docker run -d -p 3000:3000 --name node-dev $IMAGE_NAME:$DEV_TAG"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "✅ You can run integration/unit tests here"
                }
            }
        }

        stage('Tag as Test') {
            steps {
                script {
                    sh "docker tag $IMAGE_NAME:$DEV_TAG $IMAGE_NAME:$TEST_TAG"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh "docker rm -f node-dev || true"
                }
            }
        }
    }
}
