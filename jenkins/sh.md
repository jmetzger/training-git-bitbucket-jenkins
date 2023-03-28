# sh 

## Example 1 - single lines sh 

```
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo 'good good'
                sh 'pwd'
                sh 'ls -la'
                sh 'ls -la > test.txt'
                sh 'ls -la test.txt'
                sh 'cat test.txt'
            }
        }
    }
}
```

## Example 2 - multiline sh 

```
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo 'good good'
                sh'''
                  pwd
                  ls -la
                  ls -la > test3.txt
                  ls -la test2.txt
                  cat test2.txt
                '''
            }
        }
    }
}

```
