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
