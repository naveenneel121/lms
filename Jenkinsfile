pipeline {
    agent any
    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                sh 'pwd'
                sh 'whoami'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://13.201.83.135:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_076c069eda86b6055fb9114970724454f26ddcc7" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
                echo 'CODE QUALITY COMPLETED' 
            }
        }
        stage('Build LMS') {
            steps {
                echo 'Build LMS'
                sh 'pwd'
                sh 'whoami'
                sh 'cd webapp && npm install && npm run build'
                echo 'Build Completed'
            }
        }

        stage('Release LMS') {
            steps {
                script {
                    sh 'pwd'
                    sh 'whoami'
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:lms12345 --upload-file webapp/lms-${packageJSONVersion}.zip http://13.201.83.135:8081/repository/lms/"
                }
            }
        }       
        stage('Clean Up Workspace') {
            steps {
                echo 'Cleaning Workspace'
                cleanWs()
            }
        }
    }
}
