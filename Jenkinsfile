pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root'
        }
    }

    environment {
        IMAGE_NAME = "jayaseelan23/flask-demo:latest"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds-id')
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test and Build') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'pytest'
                    }
                }
                stage('Build Docker Image') {
                    steps {
                        sh 'docker build -t $IMAGE_NAME .'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([ credentialsId: 'dockerhub-creds-id', url: 'https://index.docker.io/v1/' ]) {
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }
    }
}
