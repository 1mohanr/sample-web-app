pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials-id' // Jenkins credentials ID
        DOCKER_IMAGE = "ramm978/sample-web-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/1mohanr/sample-web-app.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                        docker stop sample-web-app || true
                        docker rm sample-web-app || true
                        docker run -d -p 9090:8080 --name sample-web-app ramm978/sample-web-app:latest
                    '''
                }
            }
        }
    }
}

