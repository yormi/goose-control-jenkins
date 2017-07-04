#!/usr/bin/env groovy

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                dir server
                sh 'stack build --install-ghc'
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
