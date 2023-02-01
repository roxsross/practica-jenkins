pipeline {
    agent any
    environment {
        PRUEBA = "Jenkins-prueba"
    }

    stages {
        stage('Install Dependencias') {
            steps {
                echo 'init'
            }
        }
        stage('Unit-test') {
            steps {
                echo 'test'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'build'
            }
        }
        stage('Docker Push to Docker-hub') {
            steps {
                echo 'push'
            }
        }
        stage('Deploy to Develop') {
            steps {
                echo 'develop'
            }
        }
        stage('Notificacion') {
            steps {
                echo 'telegram/slack/discord/team'
            }
        }
    }
}