pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t samtiff/duo-deploy-flask:latest -t samtiff/duo-deploy-flask:v$BUILD_NUMBER .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push samtiff/duo-deploy-flask:latest
                docker push samtiff/duo-deploy-flask:v$BUILD_NUMBER
                '''
            }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker rmi samtiff/duo-deploy-flask:latest
                docker rmi samtiff/duo-deploy-flask:v$BUILD_NUMBER
                ''' 
            } 
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments
                kubectl rollout restart deployment flask-deployment
                '''
            }
        }
    }
}
