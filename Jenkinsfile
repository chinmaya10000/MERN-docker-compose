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
        stage('Static Code Analysis') {
            steps {
                script {
                    staticCodeAnalysis('mern/backend', 'mern/frontend')
                }
            }
        }
        stage('Unit Testing') {
            steps {
                script {
                    unitTesting('mern/backend', 'mern/frontend')
                }
            }
        }
        stage('Scan Filesystem for Vulnerabilities') {
            steps {
                scritp {
                    scanCodebase()
                }
            }
        }
        stage('build') {
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
        stage('push') {
            steps {
                script {
                    pushDockerImage(env.DOCKER_REGISTRY, env.IMAGE_VERSION)
                }
            }
        }
    }
}