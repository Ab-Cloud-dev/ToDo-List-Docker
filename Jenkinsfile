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
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'docker image ls'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}'
                sh 'docker push ${IMAGE_TAG}'


            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                    echo "Deploying Docker image ${IMAGE_TAG}"
                   sh "docker run -d -p 5000:5003 --name ${IMAGE_TAG}"
                }
            }
        }
    }
}
