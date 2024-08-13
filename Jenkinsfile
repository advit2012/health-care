pipeline {
    agent { label 'manjeet' } // All stages will run on the 'manjeet' slave node
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
                    sh 'docker build -t advit2012/healthcare13aug:v1 .'
                }
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
                sh 'docker push advit2012/healthcare13aug:v1'
            }
        }
        stage('Deploy to K8s') {
            when { branch 'master' }
            steps {
                script {
                    kubernetesDeploy(configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
