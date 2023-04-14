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
                
                script {
                    def customImage = docker.build("raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID}")
                }
                
                sh """
                echo "Success build docker image 1"
                """
            }
        }
    }   
}