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
                sh 'docker image build -t webapp-ucreativa .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag webapp-ucreativa scdel7/webapp-ucreativa:latest'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'docker-id', variable: 'dockerpwd')]) {
   sh 'docker login -u scdel7 -p ${dockerpwd} '
                sh 'docker image push scdel7/webapp-ucreativa:latest'
}
                
            }
        }
    }
}
