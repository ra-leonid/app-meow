pipeline {

  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }

  stages {
    stage('Build-Docker-Image') {
      steps {
        container('docker') {
          sh 'docker build -t ss69261/testing-image:latest .'
        }
      }
    }
    stage('Login-Into-Docker') {
      steps {
        container('docker') {
          sh 'docker login -u <docker_username> -p <docker_password>'
        }
      }
    }
    stage('Push-Images-Docker-to-DockerHub') {
      steps {
        container('docker') {
          sh 'docker push ss69261/testing-image:latest'
        }
      }
    }
  }
  post {
    always {
      container('docker') {
        sh 'docker logout'
      }
    }
  }
}
        
//         stage('Build docker image') {
//             steps {
//                 sh "printenv"

//                 withCredentials([
//                     usernamePassword(
//                         credentialsId: 'auth-dockerhub',
//                         usernameVariable: 'USERNAME',
//                         passwordVariable: 'PASSWORD')]) {
//                     sh "dockerd"
//                     sh "docker login -u ${USERNAME} -p ${PASSWORD}"
//                     sh "docker image build -t raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID} ."
//                     sh "docker image build -t raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID} ."                
//                 }               
                
//                 sh """
//                 echo "Success build docker image1"
//                 """
//             }
//         }
//     }   
// }