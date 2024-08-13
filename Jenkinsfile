pipeline {
    agent any
    stages {
        stage('Build Maven') {
            steps {
                git url: 'https://github.com/advit2012/health-care/', branch: 'master'
                sh 'mvn clean install'
            }
        }
        
        stage('Build Docker image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t advit2012/healthcare13aug:v1 .'
                }
            }
        }
        
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    echo 'Logging into Docker...'
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
                echo 'Pushing Docker image...'
                sh 'docker push advit2012/healthcare13aug:v1'
            }
        }
        
        stage('Deploy to K8s') {
            steps {
                script {
                    echo 'Deploying to Kubernetes...'
                       kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
