#!/usr/bin/env groovy

pipeline {
    agent any

    environment {
        APP_ROOT = "www/"
        APP_STATICS = "www/static/"

        SSH_URL = "server@yormi.ddns.net"
        SSH_PORT = "22222"
        SSH = "ssh -p 22222 server@yormi.ddns.net"

        SERVER_EXE_NAME = "contractor-server"
        SERVER_TARGET_PATH = "server/.stack-work/dist/x86_64-linux/Cabal-1.24.2.0/build/contractor-server/"

        INDEX_PATH = "web/dist/index.html"
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
                parallel server: {
                    echo 'Building Server..'
                    dir ('server') {
                        sh 'stack build'

                    }
                },
                web: {
                    echo 'Building Web..'
                    dir ('web') {
                        sh 'rm -rf dist'
                        sh 'mkdir dist'
                        sh 'elm make --yes Main.elm --output=dist/index.html'
                    }
                }
            }
        }

        stage ('Clean') {
            steps {
                sh "${SSH} rm -rf ${APP_ROOT}"
                sh "${SSH} mkdir -p ${APP_STATICS}"
            }
        }

        stage ('Deploy') {
            steps {
                echo 'Deploying....'
                sh "scp -P ${SSH_PORT} ${SERVER_TARGET_PATH}${SERVER_EXE_NAME} ${SSH_URL}:${APP_ROOT}"
                sh "scp -P ${SSH_PORT} ${INDEX_PATH} ${SSH_URL}:${APP_STATICS}"
            }
        }

        stage ('Restart') {
            steps {
                sh "${SSH} killall ${SERVER_EXE_NAME} || true;"
                sh "${SSH} systemctl --user restart myserver"
            }
        }
    }
}
