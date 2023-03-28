# Change variable 

```
def animal="cat"
pipeline {
  agent any
  stages {
    stage("hello") {
      steps {
        script {
          echo animal
          animal = "dog";
        }
      }
    }
    stage("goodbye") {
      steps {
        script {
          echo animal
        }
      }
    }
  }
}
```
