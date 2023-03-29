# Using docker agent 

## Running docker / docker needs to be installed on agent

```
pipeline {
    agent {
        docker { image 'node:16.13.1-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'

            }
        }
    }
}
```

## Run on specific docker agents 

```
pipeline {
    agent {
        docker { 
            image 'node:16.13.1-alpine'
            label 'linux'
            
        }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'

            }
        }
    }
}


```
