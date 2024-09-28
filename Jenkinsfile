pipeline { 
agent any
environment {     
    DOCKERHUB_CREDENTIALS= credentials('dockerhub')     
}
stage('Login to Docker Hub') {      	
    steps{                       	
	sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                		
	echo 'Login Completed'      
    }           
}  
}
