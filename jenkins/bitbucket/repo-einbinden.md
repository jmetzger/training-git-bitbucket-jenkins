# Repo einbinden 

## Walkthrough 

```
9.1. Repository erstellen. 
- bestehendes verbinden 

9.2. Repository local clonen 

9.3. Jenkinsfile in toplevel erstellen. 

Inhalt: 
```

```
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo 'good good'
                sh '''
                  pwd
                  ls -la
                  ls -la > test.txt
                '''
                
            }
        }
        
        stage('Checker') {
            steps {
              sh 'ls -la'
            }
        }
        
        stage('Archiver') {
            steps {
              archiveArtifacts artifacts: 'test.txt', followSymlinks: false
            }
        }
        
    }
}

9.4. pushen 

9.5. Job einrichten. 

Haken bei on bitbucket push setzten

9.6. Job ausführen 
```


