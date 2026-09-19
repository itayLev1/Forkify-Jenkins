pipeline {
    agent any

    environment {
        APP_NAME = 'forkify-app'
        APP_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/YOUR_USERNAME/Forkify-Jenkins.git'
            }
        }

        stage('Install dependencies') {
            steps {
                echo 'Installing app dependencies...'
                sh '''
                    cd src
                    npm ci
                '''
            }
        }

        stage('Build frontend') {
            steps {
                echo 'Building frontend with Parcel...'
                sh '''
                    cd src
                    npm run build
                '''
            }
        }

        stage('Build Docker image') {
            steps {
                echo 'Building Docker image...'
                sh '''
                    docker build -t ${APP_NAME}:${APP_TAG} .
                '''
            }
        }

        stage('Run app locally') {
            steps {
                echo 'Starting container...'
                sh '''
                    docker rm -f forkify-container || true
                    docker run -d --name forkify-container -p 8080:80 ${APP_NAME}:${APP_TAG}
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Jenkins logs.'
        }
    }
}