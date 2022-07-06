pipeline {
    environment {
        registry = "40.121.32.216:8085/chatapp"
        registryCredential = 'nexus-repo'
    }
    
    agent any
    
    tools {
        //install the maven version configured as m2 and add it to the path
        maven "mvn3"
    }
    
    stages {
        stage('pull from scm'){
            steps {
                git branch: 'main', credentialsId: 'git-lab-token', url: 'https://github.com/blazerrt86899/chat-app-private.git'
            }
        }
        stage('build it') {
            steps {
            sh 'mvn clean package'
            }
        }
        stage('docker image') {
            steps {
                script {
                  dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('docker push') {
            steps {
                script {
                  docker.withRegistry('http://40.121.32.216:8085',registryCredential) {
                      dockerImage.push()
                  }
                }
            }
        }
        
    }
}
