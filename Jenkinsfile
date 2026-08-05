apiVersion: v1
kind: Pod
spec:

  containers:

  - name: docker
    image: mikhill/jenkins-devsecops:v2
    securityContext:
      privileged: true
    tty: true
    command:
    - cat
    volumeMounts:
    - name: docker-socket
      mountPath: /var/run/docker.sock


  volumes:

  - name: docker-socket
    hostPath:
      path: /var/run/docker.sock
