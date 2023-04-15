pipeline {

    agent {
        node {
            label 'jenkins-jenkins-agent'
        }
    }

    stages {
        
        stage('Build docker image') {
            steps {
                sh "printenv"

                withCredentials([
                    usernamePassword(
                        credentialsId: 'auth-dockerhub',
                        usernameVariable: 'USERNAME',
                        passwordVariable: 'PASSWORD')]) {
                    sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                    sh "docker image build -t raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID} ."
                    sh "docker image build -t raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID} ."                
                }               
                
                sh """
                echo "Success build docker image1"
                """
            }
        }
    }   
}