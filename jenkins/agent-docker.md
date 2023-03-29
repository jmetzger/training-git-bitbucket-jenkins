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

## Using different docker images in different stages

```
pipeline {
    agent none
    stages {
        stage('Back-end') {
            agent {
                docker { image 'maven:3.9.0-eclipse-temurin-11' }
            }
            steps {
                sh 'mvn --version'
            }
        }
        stage('Front-end') {
            agent {
                docker { image 'node:16.13.1-alpine' }
            }
            steps {
                sh 'node --version'
            }
        }
    }
}
```
