pipeline {
    agent any
    stages {
        stage('Sonar Analysis') {
            steps {
			echo 'Starting Code Analysis'
			sh 'cd webapp && sudo docker run  --rm -e SONAR_HOST_URL="http://3.109.186.51:9000" -e SONAR_LOGIN="sqp_54cfd65b8591803331ec355263b224ce486142a8"  -v ".:/usr/src" sonarsource/sonar-scanner-cli:5.0 -Dsonar.projectKey=lms'
                echo 'Completed Code Analysis...'
            }
        }
	}
        stage('Build') {
            steps {
				echo 'Start Building..'
				sh sudo 'cd webapp && npm install && npm run build'
                echo 'Completed Building...'
            }
        }
		
		stage ( 'Release LMS') {
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
        stage('Deployment of LMS') {
            steps {
               script{
					def packageJson = readJSON file: 'webapp/package. json'
					def packageJSONVersion = packageJson.version
					echo "${packageJSONVersion}"
					sh "curl -u admin:lms12345 -X GET \'http://3.109.186.51:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
					sh 'sudo rm -rf /var/www/html/*'
					sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
					sh "sudo cp -r webapp/dist/* /var/www/html"
					echo 'Completed Deployment...'

					}
            }
        }
		stage('Clean up the Workstations') {
			step {
				echo 'Cleaning Up Work space'
				//Install cleanup workspace
				cleanWs () 
				}
			}
}
