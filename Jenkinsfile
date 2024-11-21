pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl create ns production || echo "------- Production Namespace Already Exists -------"
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl create ns development || echo "------- Development Namespace Already Exists -------"
                        '''
                    } else {
                        sh'echo "Unknown branch"'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main'){
                        sh '''
                        docker build -t parilvadher/duo-jenk:latest -t parilvadher/duo-jenk:v${BUILD_NUMBER} .
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t parilvadher/duo-jenk:latest -t parilvadher/duo-jenk:v${BUILD_NUMBER} .
                        '''
                    }
                }
            }
        }
        stage('Push') {
            steps {

                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        docker push parilvadher/duo-jenk:latest
                        docker push parilvadher/duo-jenk:v${BUILD_NUMBER}
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker push parilvadher/duo-jenk:latest
                        docker push parilvadher/duo-jenk:v${BUILD_NUMBER}
                        '''
                    } else {
                        sh'echo "Unknown branch"'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        kubectl apply -f ./kubernetes -n production
                        kubectl set image deployment/flask-deployment flask-container=parilvadher/duo-jenk:v${BUILD_NUMBER} -n production
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh'''
                        kubectl apply -f ./kubernetes -n development
                        kubectl set image deployment/flask-deployment flask-container=parilvadher/duo-jenk:v${BUILD_NUMBER} -n development
                        '''
                    } else {
                        sh'echo "Unknown branch"'
                    }
                }
            }
        }
    }
}
