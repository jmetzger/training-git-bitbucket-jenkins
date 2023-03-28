# Reading credentials into environment 

```
pipeline {
  agent any
  
  environment {
    DOCKER=credentials('docker-login')  
  }
  
  stages {
     stage ('build'){
       steps {
         echo "$DOCKER_USR"
         echo "$DOCKER_PSW"
         sh 'echo hallo pass: $USER_PSW'
         sh 'echo hallo usr: $USER_USR'
         sh 'env'
         
         echo "${DOCKER_USR}"
         echo "${DOCKER_PSW}"
         
         echo  "${env.DOCKER_USR}"
         
         echo "$DOCKER_USR"
         
       }    
       
       
   
     }
   
  } 
  
}


```
