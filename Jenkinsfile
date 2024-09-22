node {    
      def app          
      stage('Build image') {         
       
            app = docker.build("naveenneel121/test")    
       }     
      stage('Test image') {           
            app.inside {            
             
             sh 'echo "Tests passed"'        
            }    
        } 
      }
