#!/usr/bin/env groovy

@Library('jenkins-shared-library')_

pipeline {
    agent any
    tools {
        nodejs 'Node'
    }
    environment {
        DOCKER_REGISTRY = 'chinmayapradhan'
        IMAGE_VERSION = "${env.BUILD_NUMBER}"
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
                    installDependencies('mern/backend', 'mern/frontend')
                }
            }
        }
        stage('Scan Filesystem for Vulnerabilities') {
            steps {
                script {
                    scanCodebase()
                }
            }
        }
        stage('build Image') {
            steps {
                script {
                    buildDockerImage(env.DOCKER_REGISTRY, 'mern/backend', 'mern/frontend', env.IMAGE_VERSION)
                }
            }
        }
        stage('Image Security Scan') {
            steps {
                script {
                    imageSecurityScan("${DOCKER_REGISTRY}/backend:${IMAGE_VERSION}", "${DOCKER_REGISTRY}/frontend:${IMAGE_VERSION}")
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    pushDockerImage(env.DOCKER_REGISTRY, env.IMAGE_VERSION)
                }
            }
        }
        stage('Deploy App') {
            environment {
                DOCKER_CREDS = credentials('docker-credentials-id')
            }
            steps {
                script {
                    echo 'deploying docker image to EC2..'
                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_VERSION} ${DOCKER_CREDS_USR} ${DOCKER_CREDS_PSW}"
                    def ec2Instance = 'ec2-user@3.22.194.148'

                    sshagent(['ec2-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                    }
                }
            }
        }
    }
}