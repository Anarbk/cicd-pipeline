pipeline {
    agent any
    environment {
        // Укажи здесь свое имя пользователя Docker Hub
        DOCKER_USER = tianhewan
        IMAGE_NAME = ci-cd
    }
 
    stages {
        stage('Checkout') { // Activity 1
            steps {
                checkout scm
            }
        }
 
        stage('Build') { // Activity 2
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }
 
        stage('Test') { // Activity 3
            steps {
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }
 
        stage('Docker Build') { // Activity 4
            steps {
                sh "docker build -t ${DOCKER_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER} ."
                sh "docker tag ${DOCKER_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_USER}/${IMAGE_NAME}:latest"
            }
        }
 
        stage('Docker Push') { // Activity 5
            steps {
                // Используем ID credentials, который ты создал в Jenkins
                withCredentials([usernamePassword(credentialsId: 'docker_hub_creds_id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                    sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:latest"
                }
            }
        }
    }
}
