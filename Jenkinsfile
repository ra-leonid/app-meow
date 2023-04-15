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
                
                sh "docker login -u username --password-stdin"
                sh "docker image build -t raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID} ."                
                sh "docker image build -t raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID} ."                
                
                //script {
                //    def customImage = docker.build("raleonid/app-meow:${JOB_BASE_NAME}-${BUILD_ID}")
                //}
                
                sh """
                echo "Success build docker image"
                """
            }
        }
    }   
}