pipeline {
  agent none
  stages {
    stage('Docker Build') {
      agent any
      steps {
	sh 'hostname'
	sh 'pwd'
	sh 'cd lms/webapp/'
        sh 'docker build -t naveenneel12o/lms-fe:10.1 .'
      }
    }
  }
}
