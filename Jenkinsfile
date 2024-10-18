#!/usr/bin/env groovy

@Library('jenkins-shared-library')_

pipeline {
    agent any
    tools {
        nodejs 'Node'
    }
    environment {
        IMAGE_REGISTRY = 'chinmayapradhan'
        IMAGE_VERSION = "${env.BUILD_NUMBER}"
        SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
        stage('Clone Repo') {
            steps {
                script {
                    cloneRepo("https://github.com/chinmaya10000/MERN-docker-compose.git", "master")
                }
            }
        }
        stage('Secret Scanning with Gitleaks') {
            steps {
                script {
                    gitleaksScan('.', 'gitleaks-report.json')
                }
            }
        }
        stage('Unit Tests') {
            steps {
                script {
                    echo 'Implement unit tests if applicable.'
                    echo 'This stage is a sample placeholder'
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
        stage('SonarQube Analysis') {
            steps {
                script {
                    sonarQubeAnalysis('sonar-server', 'mern-app', 'mern-app')
                }
            }
        }
        stage('Quality Gate Check') {
            steps {
                script {
                    qualityGateCheck()
                }
            }
        }
        stage('build Image') {
            steps {
                script {
                    dir('mern/backend') {
                        buildDockerImage("${env.IMAGE_REGISTRY}/backend", env.IMAGE_VERSION)
                    }
                    dir('mern/frontend') {
                        buildDockerImage("${env.IMAGE_REGISTRY}/frontend", env.IMAGE_VERSION)
                    }
                }
            }
        }
        stage('Image Security Scan') {
            steps {
                script {
                    imageSecurityScan("${env.IMAGE_REGISTRY}/backend", env.IMAGE_VERSION, 'backend-image.json')
                    imageSecurityScan("${env.IMAGE_REGISTRY}/frontend", env.IMAGE_VERSION, 'frontend-image.json')
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    pushDockerImage("${env.IMAGE_REGISTRY}/backend", env.IMAGE_VERSION)
                    pushDockerImage("${env.IMAGE_REGISTRY}/frontend", env.IMAGE_VERSION)
                }
            }
        }
        stage('Update Deployment Files') {
            steps {
                script {
                    echo 'update file'
                }
            }
        }
    }
}