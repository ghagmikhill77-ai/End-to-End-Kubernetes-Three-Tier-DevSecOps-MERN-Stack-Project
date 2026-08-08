pipeline {

    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
  containers:
    - name: docker
      image: docker:26-dind
      securityContext:
        privileged: true
      command:
        - dockerd-entrypoint.sh
      args:
        - "--host=tcp://0.0.0.0:2375"
      env:
        - name: DOCKER_TLS_CERTDIR
          value: ""
        - name: DOCKER_HOST
          value: "tcp://localhost:2375"
'''
        }
    }

    stages {

        stage('Docker Test') {
            steps {
                container('docker') {
                    sh '''
                    sleep 15
                    docker version
                    docker ps
                    '''
                }
            }
        }

        stage('Build Backend') {
            steps {
                container('docker') {
                    dir('Application-Code/backend') {
                        sh '''
                        docker build -t mikhill/backend:v1 .
                        '''
                    }
                }
            }
        }

        stage('Build Frontend') {
            steps {
                container('docker') {
                    dir('Application-Code/frontend') {
                        sh '''
                        docker build -t mikhill/frontend:v1 .
                        '''
                    }
                }
            }
        }

        stage('Push Images') {
            steps {
                container('docker') {
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'dockerhub-creds',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )
                    ]) {
                        sh '''
                        echo "$DOCKER_PASS" | docker login \
                        -u "$DOCKER_USER" \
                        --password-stdin

                        docker push mikhill/backend:v1
                        docker push mikhill/frontend:v1
                        '''
                    }
                }
            }
        }
    }
}
