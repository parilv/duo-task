pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t parilvadher/duo-jenk:latest -t parilvadher/duo-jenk:v${BUILD_NUMBER} .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push parilvadher/duo-jenk:latest
                docker push parilvadher/duo-jenk:v${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh'''
                kubectl apply -f ./kubernetes
                kubectl set image deployment/flask-deployment flask-container=parilvadher/duo-jenk:v${BUILD_NUMBER}
                '''
            }
        }
    }
}
