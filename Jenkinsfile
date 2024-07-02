pipeline {
    agent any
    environment {
        // Define your Docker Hub credentials ID here
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Build Maven') {
            steps {
                sh 'pwd'
                sh 'mvn clean install package'
            }
        }
        stage('Copy Artifacts') {
            steps{
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def customImage = docker.build("saqlainpathan9/petclinic:${env.BUILD_NUMBER}", "./docker")
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS)
                    customImage.push('latest')
                }
            }
        }
    }
}
