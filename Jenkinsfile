pipeline {
    agent any
    tools{
    maven 'MVN'
    dockerTool 'DOCKER'}
     environment {

            // Define Docker Hub credentials ID
            DOCKERHUB_CREDENTIALS_ID = 'Docker_hub'
            // Define Docker Hub repository name
            DOCKERHUB_REPO = 'joonaron/inclassweek7'
            // Define Docker image tag
            DOCKER_IMAGE_TAG = 'latest'
        }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ADirin/SEP1_Week7_Spring2025_Inclass_solution.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
        }
        stage('Publish Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage('Publish Coverage Report') {
            steps {
                jacoco()
            }
        }

         stage('Build Docker Image') {
            steps {
                // Build Docker image
                script {
                    sh 'docker context use default'
                    docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                // Push Docker image to Docker Hub
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
