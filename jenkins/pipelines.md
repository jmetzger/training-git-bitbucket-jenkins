# Pipelines 

## Declarative vs. Scripted

### Übersicht 

![Schaubild](https://www.jenkins.io/doc/book/resources/pipeline/realworld-pipeline-flow.png)


### Example Scripted 

```
// Scripted //
node {
    stage('Example') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }
}
```



### Example Declarative

 * Daten werden in nicht-Docker pipelines von Schritt zu Schritt vererbt. 

```
pipeline { 
    agent any 
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps { 
                sh 'make' 
            }
        }
        stage('Test'){
            steps {
                sh 'make check'
                junit 'reports/**/*.xml' 
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish'
            }
        }
    }
}
```
