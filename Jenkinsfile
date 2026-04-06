pipeline {
    agent any

    environment {
        CONTAINER_NAME = "nestjs-app"
        IMAGE_NAME = "nesths-image"
        EMAIL = "faiyazkhanpc5676@gmail.com"
        PORT = "3001"
    }

    stages {
        stage('Clone Repo'){
            steps{
                git branch: 'main', url: 'https://github.com/techfaiyaz5/cicd-with-docker-jenkins-aws.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Sudo lagaya hai taaki permission issue na aaye
                sh "sudo docker build -t ${IMAGE_NAME} ."
            }
        }
        
        stage('Stop & Remove Previous Container') {
            steps {
                sh """
                    sudo docker stop ${CONTAINER_NAME} || true
                    sudo docker rm ${CONTAINER_NAME} || true
                """
            }
        }
        
        stage('Docker Container Run') {
            steps {
                // Port variable ko curly braces mein dala hai
                sh "sudo docker run -d -p ${PORT}:${PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
        
        stage('Send Email Notification') {
            steps {
                emailext(
                    subject: "NestJS App Deployed Successfully on EC2!",
                    body: "Your Nest JS app is Deployed! http://13.53.206.92:${PORT}/",
                    to: "${env.EMAIL}"
                )
            }
        }
    }
}