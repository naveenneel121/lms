pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'naveenneel12o/lms-frontend'  // Replace with your desired image name
        DOCKER_REGISTRY = 'https://hub.docker.com/'  // Optional: Replace with your Docker registry URL
    }
    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                sh 'pwd'
                sh 'whoami'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://3.110.207.106:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_842ac7e930dee6ab4ba2e0196c1d3cee4f725fc7" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
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
                    sh "curl -v -u admin:lms12345 --upload-file webapp/lms-${packageJSONVersion}.zip http://3.110.207.106:8081/repository/lms/"
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
                    sh "curl -u admin:lms12345 -X GET \'http://3.110.207.106:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
                    sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
                    //sh "sudo cp -r webapp/* /home/ubuntu/lms/webapp/"
                    sh "sudo ls /var/lib/jenkins/workspace/lms-test/webapp/"
                    sh "sudo cp -r webapp/* /var/lib/jenkins/workspace/lms-test/webapp/"
                }
            }
        }                
       // stage('Checkout') {
       //     steps {
        //        // Pull the code from your source control
        //        //checkout scm
        //    }
       // }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'cd webapp'
                    // Build the Docker image using the Dockerfile
                    sh 'whoami'
                    sh 'pwd'
                    sh 'cd /var/lib/jenkins/workspace/lms-test/webapp'
                    sh 'ls'
                    //sh 'cat Dockerfile'
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} -f /var/lib/jenkins/workspace/lms-test/webapp/Dockerfile ."
                }
            }
        }

        stage('Test Docker Image') {  // Optional: You can skip this if you don’t need to test the image.
            steps {
                script {
                    // Run the Docker image for testing
                    sh 'whoami'
                    sh 'pwd'
                    sh "docker run --rm ${DOCKER_IMAGE}:${env.BUILD_ID}"
                }
            }
        }

        stage('Push Docker Image') {  // Optional: Push the image to a Docker registry
            steps {
                script {
                    // Login to Docker registry
                    sh 'whoami'
                    sh 'pwd'
                    sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASSWORD} ${DOCKER_REGISTRY}"

                    // Tag and push the Docker image
                    sh 'whoami'
                    sh 'pwd'
                    sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_ID} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_ID}"
                    sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_ID}"
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
