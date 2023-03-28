# Define environment vars within pipeline 


pipeline {
    agent any
    // for all stages 
    environment { 
        CC = 'clang'
    }
    stages {
    
        stage('Example') {
            // for a specific stage
            environment { 
                MY_STAGE_ENV_VAR = 'Stage env var is this' 
            }
            steps {
                sh 'printenv'
            }
        }
        
        stage('Only toplevel stage'){
            steps {
                sh 'printenv'
                sh 'env'
            }
        
        }
        
    }
}
