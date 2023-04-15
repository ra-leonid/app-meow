pipeline {
    agent {
        kubernetes {
            label 'img'
            yaml """
kind: Pod
metadata:
  name: img
  annotations:
    container.apparmor.security.beta.kubernetes.io/img: unconfined  
spec:
  containers:
  - name: img
    workingDir: /home/jenkins
    image: r.j3ss.co/img
    imagePullPolicy: Always
    securityContext:
        rawProc: true
        privileged: true
    command:
    - cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: .docker/config.json
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