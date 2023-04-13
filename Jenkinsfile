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
                
                sh "docker image build -t raleonid/app-meow:0.0.2 ."
                
                sh """
                echo "Success build docker image 1"
                """
            }
        }
    }   
}