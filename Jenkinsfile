#!/usr/bin/env groovy

pipeline {
    agent any
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    stages {
        stage('build war file') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn package'
                }
            }
        }
        stage('build docker image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub ', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                    sh 'sudo systemctl start docker'
                    sh 'sudo docker build -t devopstrainer/myrepoprivate:jenkinsjob .'
                    sh 'echo $PASS | sudo docker login -u $USER --password-stdin'
                    sh 'sudo docker push devopstrainer/myrepoprivate:jenkinsjob'
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }
}
