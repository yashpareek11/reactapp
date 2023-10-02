pipeline {
    agent any
    tools {nodejs "node"}
    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yashpareek11/reactapp.git']])
            }
        }
        stage('Build') {
            steps {
				sh 'npm install'
            }
        }
        stage('Docker Image') {
            steps {
				sh 'docker build -t yashpareek99/pipeline-image .'
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
              withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                  sh 'docker login -u yashpareek99 -p ${dockerhubpwd}'
                  sh 'docker push yashpareek99/pipeline-image'

              }
            }
        }
    }
}
