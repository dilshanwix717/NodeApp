pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/dilshanwix717/NodeApp.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                sh 'docker build -t dilshanwix/nodeapp-test:%BUILD_NUMBER% .'//use "sh" for mac and linux, use "bat" for windows
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-pwd', variable: 'test-pwd')]) {
                script {
                        sh "docker login -u dilshanwix -p ${test-pwd}"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push dilshanwix/nodeapp-test:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
