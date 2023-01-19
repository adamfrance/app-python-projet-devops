pipeline {
    environment {
        registry = 'user/projet'
        registryCredential = 'dockerhub_id'
        dockerImage = ''
    }
    agent any
    stages {
        node {
            stage('SCM') {
                checkout scm
            }
            stage('SonarQube Analysis') {
                def scannerHome = tool 'SonarqubeScanner'
                withSonarQubeEnv() {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Run our image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.run()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
