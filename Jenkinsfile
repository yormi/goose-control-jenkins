#!/usr/bin/env groovy

pipeline {
    agent any

    environment {
        APP_ROOT = "www/"
        SSH_URL = "server@yormi.ddns.net"
        SSH_PORT = "22222"
        SSH = "ssh -p 22222 server@yormi.ddns.net"
        SERVER_EXE_NAME = "contractor-server"
        SERVER_TARGET_PATH = "server/.stack-work/dist/x86_64-linux/Cabal-1.24.2.0/build/contractor-server/"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: '7dfbf42f-228f-4d05-af6e-5e7061d055f2', url: 'https://github.com/yormi/contractor.git'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
                dir ('server') {
                    sh 'stack test'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building..'
                dir ('server') {
                    sh 'stack build'
                }
            }
        }

        stage ('Deploy') {
            steps {
                echo 'Deploying....'
                sh " ${SSH} rm ${APP_ROOT}${SERVER_EXE_NAME}"
                sh "scp -P ${SSH_PORT} ${SERVER_TARGET_PATH}${SERVER_EXE_NAME} ${SSH_URL}:${APP_ROOT}"
            }
        }

        stage ('Restart') {
            steps {
                sh "${SSH} killall ${SERVER_EXE_NAME} || true;"
                sh "${SSH} www/${SERVER_EXE_NAME} & disown"
            }
        }
    }
}
