pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        kubernetes {
          label 'jenkinsrun'
          defaultContainer 'builder'
          yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: builder
    image: gcr.io/kaniko-project/executor:latest
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
  volumes:
    - name: kaniko-secret
      secret:
        secretName: regcred
        items:
        - key: .dockerconfigjson
          path: config.json
"""
        }
      }
    steps {
          script {
            sh "/kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --destination=index.docker.io/v2/raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID}-kaniko"
        }
      } //steps
    } //stage(build)
  }
}