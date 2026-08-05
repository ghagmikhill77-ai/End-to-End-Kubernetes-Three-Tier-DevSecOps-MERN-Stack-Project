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

  volumeMounts:
  - name: docker-storage
    mountPath: /var/lib/docker
