pipeline {

    agent {
        node {
            label 'jenkins-cloud-agent'
        }
    }

    stages {
        
        stage('Build docker image') {
            steps {
                sh """
                echo "Success build docker image"
                """
            }
        }
    }   
}