pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mabd007/jenkins-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.GIT_COMMIT}"
        
    }

    
    stages {

        stage('Setup') {
            steps {
                sh "python3 -m pip install -r requirements.txt"
            }
        }
        stage('Test') {
            steps {
                sh "python3 -m pytest"
            }
        }

        stage('Login to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'}
                echo 'Login successfully'
            }
        }

        stage('Build Docker Image')
        {
            steps
            {
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image build successfully"
                sh 'docker image ls'
                
            }
        }

        stage('Push Docker Image')
        {
            steps
            {
                sh 'docker push ${IMAGE_TAG}'
                echo "Docker image push successfully"
            }
        }
		stage('Deploy Docker Image') {
            steps {
                script {
                    echo "Deploying Docker image ${IMAGE_TAG}"
                    sh "docker run -d ${IMAGE_TAG}"
                }
            }
        }	
    }
}
