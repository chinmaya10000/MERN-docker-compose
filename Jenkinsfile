pipeline {
    agent {
        docker {
            image 'node:18.9.1'
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Mount Docker socket for building images
        }
    }
    environment {
        DOCKER_REGISTRY = 'chinmayapradhan'
        IMAGE_VERSION = "${env.BUILD_ID}-${env.GIT_COMMIT}"
    }

    stages (
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
    )
}