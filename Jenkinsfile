pipeline {
    agent any
    environment{
        APPNAME = 'node-app-demo'
        REGISTRY = 'roxsross12'
        DOCKER_HUB_LOGIN = credentials('docker-hub')
        EC2INSTANCE = 'ec2-user@18.209.229.162'
    }
    stages { // el principal donde se arman la tuberia 
        //CI
        stage('Init') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                echo "Init"
                sh 'npm install'
            }
        }
        stage('Test') {
            agent{
                docker {
                    image 'node:erbium-alpine'
                    args '-u root:root'
                }
            }
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh 'npm run test'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t $APPNAME:$BUILD_NUMBER .'
                sh 'docker tag $APPNAME:$BUILD_NUMBER $REGISTRY/$APPNAME:$BUILD_NUMBER'

            }
        }

        stage('Push to Registry') {
            steps {
                sh 'docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW'
                sh 'docker push $REGISTRY/$APPNAME:$BUILD_NUMBER'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy to EC2'
                sh ("sed -i -- 's/REGISTRY/$REGISTRY/g' docker-compose.yaml")
                sh ("sed -i -- 's/APPNAME/$APPNAME/g' docker-compose.yaml")
                sh ("sed -i -- 's/TAG/$BUILD_NUMBER/g' docker-compose.yaml")
                sshagent(['shh-ec2']){
                    sh 'scp -o StrictHostKeyChecking=no docker-compose.yaml ${EC2INSTANCE}:/home/ec2-user'
                    sh 'ssh ${EC2INSTANCE} ls -la'
                    sh 'ssh ${EC2INSTANCE} docker-compose up -d'
                }
            }
        }
        stage('Notificaction') {
            steps {
                echo 'Telegram/slack/discord/team-...'
            }
        } 
    }
}