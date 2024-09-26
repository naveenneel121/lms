pipeline {
    agent any
    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                sh 'pwd'
                sh 'whoami'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://13.127.83.91:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_842ac7e930dee6ab4ba2e0196c1d3cee4f725fc7" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
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
                    sh "curl -v -u admin:lms12345 --upload-file webapp/lms-${packageJSONVersion}.zip http://13.127.83.91:8081/repository/lms/"
                }
            }
        } 

        stage ('Pull the dist from Nexus') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJson.version
                    echo "${packageJSONVersion}" 
                    sh 'sudo rm -rf /home/ubuntu/lms/webapp/lms.zip'
                    sh "curl -u admin:lms12345 -X GET \'http://13.127.83.91:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
                    sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/* /home/ubuntu/lms/webapp/"
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
