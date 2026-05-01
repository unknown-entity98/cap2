pipeline {
    agent any

    environment {
        IMAGE_NAME = "blackadam60091/webapp"
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t webapp .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag webapp $IMAGE_NAME:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
