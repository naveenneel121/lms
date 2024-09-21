pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'cat /etc/os-release'
            }
        }
        stage('Test') {
            steps {
                sh 'df -h'
            }
        }
        stage('Deploy') {
            steps {
                sh 'pwd'
            }
        }
    }
}
