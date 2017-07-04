#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'

                dir ('server') {
                  sh 'pwd'
                  sh 'pwd && stack build'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'stack test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
