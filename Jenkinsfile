pipeline {
    agent any

    environment {
        registry = "sc0rpion/my-node-app"
        registryCredential = 'dockerhub'
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/GameZoneHacker/node-js-dummy-test.git'
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        def dockerImage = docker.build("${registry}")
                    }
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // Example using Trivy for security scan
                    sh "docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image ${registry}"
                }
            }
        }
        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        def dockerImage = docker.image("${registry}")
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                sh "docker rmi ${registry}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
