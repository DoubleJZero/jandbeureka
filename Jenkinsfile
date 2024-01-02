pipeline {
    agent any
    environment {
        DOCKER = 'sudo docker'
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
                echo 'Checkout Scm'
            }
        }

        stage('Build image') {
            steps {
                sh 'ls -al'
                sh 'chmod +x ./gradlew'
                sh './gradlew build'
                sh 'docker build -t jandb:msaeureka .'
                echo 'Build image...'
            }
        }

        stage('Remove Previous image') {
            steps {
                script {
                    try {
                        sh 'docker stop eurekaService'
                        sh 'docker rm eurekaService'
                    } catch (e) {
                        echo 'fail to stop and remove container'
                    }
                }
            }
        }
        stage('Run New image') {
            steps {
                sh 'docker run --name eurekaService -d -p 8761:8761 -e USE_PROFILE=dev jandb:msaeureka'
                echo 'Run New member image'
            }
        }
    }
}