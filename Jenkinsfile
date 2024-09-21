pipeline {
    agent any
    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://3.109.186.51:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_d076efa96018ac4e37800c14536e7e93b977ad1b" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
                echo 'CODE QUALITY COMPLETED' 
            }
        }
        stage('Build LMS') {
            steps {
                echo 'Build LMS'
                sh 'cd webapp && npm install && npm run build'
                echo 'Build Completed'
            }
        }

        stage('Release LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}"
                    sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                    sh "curl -v -u admin:lms12345 --upload-file webapp/lms-${packageJSONVersion}.zip http://3.109.186.51:8081/repository/lms/"
                }
            }
        }

        stage('Deploy LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    sh "curl -u admin:lms12345 -X GET \'http://3.109.186.51:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
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
