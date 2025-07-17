pipeline {
    agent any
    environment {
        IMAGE_NAME = 'mabd007/jenkins-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.GIT_COMMIT}"
        DOCKERHUB_CREDENTIALS = credentials('948e07d8-0f61-4e2d-9ccb-98e7518dffdf')
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'sudo docker image ls'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '948e07d8-0f61-4e2d-9ccb-98e7518dffdf', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'echo $DOCKER_PASSWORD | sudo docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'sudo docker push ${IMAGE_TAG}'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                    echo "Deploying Docker image ${IMAGE_TAG}"
                    sh "sudo docker run -d ${IMAGE_TAG}"
                }
            }
        }
    }
}
