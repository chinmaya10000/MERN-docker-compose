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
        stage('Clone Repo') {
            steps {
                script {
                    cloneRepo("https://github.com/chinmaya10000/MERN-docker-compose.git", "master")
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
        stage('Update Deployment Files') {
            steps {
                script {
                    updateK8sDeployments(env.DOCKER_REGISTRY, 'kubernetes/backend.yaml', 'kubernetes/frontend.yaml', env.IMAGE_VERSION)
                }
            }
        }
    }
}