pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: mikhill/jenkins-devsecops:v1
    command:
    - cat
    tty: true
    securityContext:
      privileged: true
"""
            defaultContainer 'docker'
        }
    }

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
        DOCKERHUB_USERNAME = 'mikhill'
        FRONTEND_IMAGE = 'mikhill/frontend'
        BACKEND_IMAGE = 'mikhill/backend'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                sh '''
                git --version
                docker --version
                helm version
                kubectl version --client
                '''
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t $BACKEND_IMAGE:v1 .'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t $FRONTEND_IMAGE:v1 .'
                }
            }
        }

        stage('Push Images') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $BACKEND_IMAGE:v1
                    docker push $FRONTEND_IMAGE:v1
                    '''
                }
            }
        }
    }
}
