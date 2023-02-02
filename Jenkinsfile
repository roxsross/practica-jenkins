pipeline {
    agent any
    environment {
        PRUEBA = "Jenkins-prueba"
        EC2INSTANCEDEV = "ec2-user@3.237.69.190"
        REGISTRY = "roxsross12"
        APPNAME  = "node-app"
        IMAGE     = "node-app-develop"
        VERSION  = "1.0.0"
        DOCKER_HUB_LOGIN = credentials('docker-hub')
    }

    stages {
        stage('Install Dependencias') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Unit-test') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm run test'
            }
        }
        stage('Docker Build') {
            steps {
                sh ' docker build -t $REGISTRY/$APPNAME:$VERSION .'
            }
        }
        stage('Docker Push to Docker-hub') {
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh 'docker push $REGISTRY/$APPNAME:$VERSION' 
            }
        }
        stage('Deploy to Develop') {
            steps {
                echo 'DEPLOY'
                sh ("sed -i -- 's/IMAGE/$IMAGE/g' docker-compose.yaml")
                sh ("sed -i -- 's/REGISTRY/$REGISTRY/g' docker-compose.yaml")
                sh ("sed -i -- 's/APPNAME/$APPNAME/g' docker-compose.yaml")
                sh ("sed -i -- 's/VERSION/$VERSION/g' docker-compose.yaml")
                sshagent(['ssh-server']){
                    sh 'scp -o StrictHostKeyChecking=no docker-compose.yaml $EC2INSTANCEDEV:/home/ec2-user'
                    sh 'ssh $EC2INSTANCEDEV docker-compose up -d'
                }
            }
        }
        stage('Notificacion') {
            steps {
                sh 'chmod +x ./automation/telegram.sh'
                sh './automation/telegram.sh'
            }
        }
    }
}