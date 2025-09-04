pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub-cred')  
    }

    stages {
        stage('Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rk4027-N/httpd.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t nani4027/httpd:v4 .'
            }
        }
        stage('Trivy') {
            steps {
                sh 'trivy image nani4027/httpd:v4'
            }
        }
        stage('dockerhub') {
            steps {
                sh '''
                  echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                  docker push nani4027/httpd:v4
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -itd --name webserver -p 3000:80 nani4027/httpd:v4'
            }
        }
    }
}
