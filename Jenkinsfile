pipeline {
    agent any
    tools {
        nodejs 'Node'
    }
    environment {
        DOCKER_REGISTRY = 'chinmayapradhan'
        IMAGE_VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
    }

    stages {
        stage('Clone repo') {
            steps {
                script {
                    git 'https://github.com/chinmaya10000/MERN-docker-compose.git'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    dir('mern/backend') {
                        sh 'npm install'
                    }
                    dir('mern/frontend') {
                        sh 'npm install'
                    }
                }
            }
        }
    }
}