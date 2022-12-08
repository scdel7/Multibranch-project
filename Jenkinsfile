pipeline {
    agent {
        label 'worker-dev'
    }

    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -D skipTests clean install package'
            }
        }

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker image build -t java-webapp .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag java-webapp korinrovira/java-webapp:latest'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'docker-id', variable: 'dockerpwd')]) {
                  sh 'docker login -u korinrovira -p ${dockerpwd}'
                sh 'docker image push korinrovira/java-webapp:latest'
            }
        }
    }
