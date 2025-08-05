pipeline { 
    agent any 
    environment { 
        IMAGE_NAME = "jayaseelan23/flask-demo:latest" 
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds-id') 
    } 
    stages { 
        stage('Clone') { steps { git 'https://github.com/jayaseelan-r-v/flask-ci-cd-demo.git' } } 
        stage('Install Dependencies') { steps { sh 'pip install -r requirements.txt' } } 
        stage('Test and Build') { 
            parallel { 
                stage('Build Docker Image') { steps { sh 'docker build -t $IMAGE_NAME .' } } 
            } 
        } 
        stage('Push to DockerHub') { 
            steps { 
                withDockerRegistry([ credentialsId: 'dockerhub-creds-id' ]) { 
                    sh 'docker push $IMAGE_NAME' 
                } 
            } 
        } 
    } 
} 
