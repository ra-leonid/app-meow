pipeline {
  agent {
    kubernetes {
      inheritFrom 'default'
      defaultContainer 'deploy'
    }
  }
  stages {
    //Build container image
    stage('Build') {
      steps {
        sh "printenv"
        container('deploy') {
          script {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-auth') {
              def pathTag = "raleonid/app-meow:${JOB_BASE_NAME}"
              if(env.TAG_NAME == null || env.TAG_NAME.length() == 0) {
                pathTag = "${pathTag}-${BUILD_ID}"
              }
              //build the image
              def customImage = docker.build(pathTag)
              //upload it to the registry
              customImage.push()
            }
          }
        }
      }
    }
    stage('Deploy') {
      when {
          expression { env.TAG_NAME != null && env.TAG_NAME.length() > 0 }
      }
      steps {
        container('deploy') {
          // withKubeConfig([credentialsId: 'token-k8s-sa', namespace: "stage"]) {
          withKubeConfig([credentialsId: 'token-k8s-sa']) {
            sh "helm upgrade --install app-meow deploy --set image.tag=${TAG_NAME}"
          }
        }
      }
    }
  }
}