pipeline {
    agent any

    tools {
        maven 'maven-3.5.2'
    }

    environment {
        registry = "maryemelheni/devops-demo"
        dockerImage = ''
    }

    triggers {
        pollSCM('*/5 * * * *')
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/meryemelheni/Jenkins-Demo.git'
            }
        }

        stage('Build App') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Deploy Docker container') {
            steps {
                sh "docker stop demo-jenkins || true && docker rm demo-jenkins || true"
                sh "docker run --name demo-jenkins -d -p 2222:2222 $registry:$BUILD_NUMBER"
            }
        }
    }

    post {
        success { echo 'Pipeline execution successful!' }
        failure { echo 'Pipeline execution failed.' }
    }
}
