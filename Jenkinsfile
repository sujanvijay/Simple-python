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

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'Docker_CRED',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh '''
                    echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                    docker push $DOCKER_IMAGE:v1
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
