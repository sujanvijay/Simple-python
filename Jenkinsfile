pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sujanvijay/simple-python-app"
    }

    stages {

        stage('Clone Code') {
    steps {
        git url: 'https://github.com/sujanvijay/Simple-python.git',
            branch: 'main',
            credentialsId: 'sujan'
          }
      }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:v1 .'
            }
        }

        stage('DockerHub Login') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'Docker_CRED',
            usernameVariable: 'USERNAME',
            passwordVariable: 'PASSWORD'
        )]) {
            sh '''
            docker login -u "$USERNAME" -p "$PASSWORD"
            docker push sujanvijay/simple-python-app:v1
            '''
        }
    }
}

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
