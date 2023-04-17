pipeline {
  agent none
  stages {
    //Build container image
    stage('Build') {
      agent {
        kubernetes {
          label 'jenkinsrun'
          defaultContainer 'dind'
          yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: dind
    image: docker:18.05-dind
    securityContext:
      privileged: true
    volumeMounts:
      - name: dind-storage
        mountPath: /var/lib/docker
  volumes:
    - name: dind-storage
      emptyDir: {}
"""
        }
      }
      steps {
        sh "printenv"
        container('dind') {
          script {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-auth') {
              //build the image
              def customImage = docker.build("raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID}")
              //upload it to the registry
              customImage.push()
              println "test"
            }
          }
        }
        sh "kubectl version"
      }
    }
  }
}