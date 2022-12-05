pipeline {
    agent {
        label 'nodo-1'
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
                sh 'docker login -u korinrovira -p &t33lD00r928+'
                sh 'docker image push korinrovira/java-webapp:latest'
            }
        }
    }
