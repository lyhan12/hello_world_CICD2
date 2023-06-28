pipeline {
    agent any
    environment {
        DOCKERHUB = credentials("dockerhub")
        GITHUB_REPO="https://github.com/lyhan12/hello_world_CICD2"
        DOCKER_REPO="lyhan12/hello_world_server2"
    }

    stages {
        stage('Check Out') {
            steps {
                echo 'check out'
                git branch: 'main',
                    credentialsId: 'github', // For Private Repository
                    url: "$GITHUB_REPO"
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t hello_world .'
            }
        }
        stage('Docker Upload') {
            steps {
                echo "upload"

                sh 'docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW'
                sh 'docker tag hello_world $DOCKER_REPO'
                sh 'docker push $DOCKER_REPO'
                sh 'docker logout'
            }
        }
    }
    post {
        success {
            slackSend(
                channel: '#일반',
                message: "${env.BUILD_NUMBER} ${env.JOB_NAME}")
        }
    }
}
