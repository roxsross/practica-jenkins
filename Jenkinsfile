pipeline {
    agent any
    environment {
        PRUEBA = "Jenkins-prueba"
        EC2INSTANCEDEV = "ec2-user@3.237.69.190"
        REGISTRY = "roxsross12"
        APPNAME  = "node-app"
        NAME     = "node-app-develop"
        DOCKER_HUB_LOGIN = credentials('docker-hub')
    }

    stages {
        stage('Install Dependencias') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    arg '-u root:root'
                }
            }
            steps {
                sh 'npm install'
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