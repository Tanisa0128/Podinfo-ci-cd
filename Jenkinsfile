pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') 
        IMAGE_NAME = 'tanisaverma/podinfo-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning Git repository...'
                            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

       
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Tag as latest') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").tag('latest')
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and push complete!'
        }
        failure {
            echo ' Build failed.'
        }
    }
}
