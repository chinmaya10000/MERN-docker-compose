@Library('jenkins-shared-library')_

pipeline {
    agent any
    tools {
        nodejs 'Node'
    }
    environment {
        DOCKER_REGISTRY = 'chinmayapradhan'
    }
    /*parameters {
        string(name: 'FRONTEND_DOCKER_TAG', defaultValue: '', description: 'Setting docker image for latest push')
        string(name: 'BACKEND_DOCKER_TAG', defaultValue: '', description: 'Setting docker image for latest push')
    }*/

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
                    installDependencies('mern/backend', 'mern/frontend')
                }
            }
        }
        stage('build') {
            steps {
                script {
                    dir('mern/backend') {
                        sh "docker build -t ${DOCKER_REGISTRY}/backend ."
                    }
                    dir('mern/frontend') {
                        sh "docker build -t ${DOCKER_REGISTRY}/frontend ."
                    }
                }
            }
        }
        stage('push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-repo', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                        sh "docker push $DOCKER_REGISTRY/backend:latest"
                        sh "docker push $DOCKER_REGISTRY/frontend:latest"
                    }
                }
            }
        }
    }
}