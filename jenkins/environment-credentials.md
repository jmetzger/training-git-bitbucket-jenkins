# Reading credentials into environment 

```
pipeline {
  agent any
  
  environment {
    USER=credentials('access_user')  
  }
  
  stages {
     stage ('build'){
       steps {
         echo "$USER_PSW"
         echo "$USER_USR"
         sh 'echo hallo pass: $USER_PSW'
         sh 'echo hallo usr: $USER_USR'
         sh 'env'
         
       }    
       
       
   
     }
   
  } 
  
}


```
