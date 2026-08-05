pipeline {

    agent {
        kubernetes {

            yaml '''
apiVersion: v1
kind: Pod

spec:

  containers:

  - name: docker
    image: docker:26-dind

    command:
    - dockerd-entrypoint.sh

    args:
    - "--host=tcp://0.0.0.0:2375"

    securityContext:
      privileged: true

    env:

    - name: DOCKER_TLS_CERTDIR
      value: ""

    - name: DOCKER_HOST
      value: "tcp://localhost:2375"


    volumeMounts:

    - name: docker-storage
      mountPath: /var/lib/docker


  volumes:

  - name: docker-storage
    emptyDir: {}

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
}
