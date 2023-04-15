pipeline {
    agent {
        kubernetes {
            label 'img'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: img
  name: img
  annotations:
    container.apparmor.security.beta.kubernetes.io/img: unconfined
spec:
  securityContext:
    runAsUser: 1000
  initContainers:
    # This container clones the desired git repo to the EmptyDir volume.
    - name: git-clone
      image: r.j3ss.co/jq
      args:
        - git
        - clone
        - --single-branch
        - --
        - https://github.com/jessfraz/dockerfiles
        - /repo # Put it in the volume
      securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
      volumeMounts:
        - name: git-repo
          mountPath: /repo
  containers:
  - image: r.j3ss.co/img
    imagePullPolicy: Always
    name: img
    resources: {}
    workingDir: /repo
    command:
    - img
    - build
    - -t
    - irssi
    - irssi/
    securityContext:
      rawProc: true
    volumeMounts:
    - name: cache-volume
      mountPath: /tmp
    - name: git-repo
      mountPath: /repo
  volumes:
  - name: cache-volume
    emptyDir: {}
  - name: git-repo
    emptyDir: {}
  restartPolicy: Never
"""
        }
    }
    stages {
        // stage('Checkout') {
        //     steps {
        //         git 'https://github.com/joostvdg/cat.git'
        //     }
        // }
        // stage('Build') {
        //     steps {
        //         container('golang') {
        //             sh './build-go-bin.sh'
        //         }
        //     }
        // }
        stage('Make Image') {
            steps {
                container('img') {
                    sh 'mkdir cache'
                    sh 'img build -s ./cache -f Dockerfile -t raleonid/app-meow .'
                }
            }
        }
    }
}