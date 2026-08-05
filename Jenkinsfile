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
    securityContext:
      privileged: true
    command:
    - dockerd-entrypoint.sh
    args:
    - "--host=tcp://0.0.0.0:2375"
    - "--host=unix:///var/run/docker.sock"

    env:
    - name: DOCKER_TLS_CERTDIR
      value: ""

    volumeMounts:
    - name: docker-storage
      mountPath: /var/lib/docker


  - name: jnlp
    image: jenkins/inbound-agent:latest


  volumes:

  - name: docker-storage
    emptyDir: {}

'''
    }
}

}


stages {


stage('Docker Test') {

steps {

container('docker') {

sh '''

docker version
docker ps

'''

}

}

}


stage('Build Backend') {

steps {

container('docker') {

dir('backend') {

sh '''

docker build -t mikhill/backend:v1 .

'''

}

}

}

}


}

}
