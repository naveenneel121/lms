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
                sh 'dh -f'
            }
        }
        stage('Deploy') {
            steps {
                sh 'free'
            }
        }
    }
}
