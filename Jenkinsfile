pipeline {
  agent any
  tools {
    nodejs 'nodejs-22-6-0'
  } 
  stages {
    stage("Dependencies installation"){
      steps {
        sh 'npm install --no-default'
      }
    }
    
  }
}
