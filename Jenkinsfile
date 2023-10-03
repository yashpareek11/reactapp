pipeline {
    
    agent any
    tools{ nodejs "node"}
    
    stages {
        stage('Checkout') {
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yashpareek11/reactapp.git']])
            }
        }
        stage('Build') {
            steps{
                sh 'npm install'
            }
        }
        stage('Docker build image') {
            steps{
                sh 'docker build -t yashpareek99/reactjs-image .'
            }
        }
        stage('Push to Docker Hub') {
            steps{
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpass')]) {
                    sh 'docker login -u yashpareek99 -p ${dockerhubpass}'
                    sh 'docker push yashpareek99/reactjs-image'
                }
            }
        }
        stage('Deploy on K8s cluster') {
            steps{
                script {
                    kubernetesDeploy (configs: 'deployment.yaml,service.yaml', kubeconfigId: 'k8spwd')
                }
            }
        }
        
    }
  
}
