pipeline {
  agent any
  tools {
    nodejs 'nodejs-22-6-0'
  }

  stages {
    stage("Dependencies installation") {
      steps {
        sh 'npm install --no-default'
      }
    }

    stage('Dependency Scanning') {
      parallel {
        stage("NPM Dependency Audit") {
          steps {
            sh '''
              npm audit --audit-level=critical
              echo $?
            '''
          }
        }

        stage("OWASP Dependency Check") {
          steps {
            script {
                echo "Running OWASP Dependency Check..."
                // Run Dependency-Check with API key
                dependencyCheck additionalArguments: ''' 
                    -o './'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint
                ''', odcInstallation: 'OWASP-DepCheck-10'
              }
            }
          }
        }
      }
    }
  post {
    failure {
      echo 'Build failed due to dependency scanning issues.'
    }
    success {
      echo 'Dependency scanning completed successfully.'
    }
  }
}
