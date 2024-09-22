pipeline {
  agent { dockerfile true}
  stages {
    stage('Docker Build') {
      agent any
      steps {
	sh 'node --version'
      }
    }
  }
}
