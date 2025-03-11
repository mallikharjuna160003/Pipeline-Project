pipeline {
  agent any
  tools {
    nodejs 'nodejs-22-6-0'
  }
  environment {
    NVD_API_KEY = 'c89da9be-83e0-4e47-9809-bbe41ef076a7'
  }

  stages {
    stage("Dependencies installation") {
      steps {
        script {
          // Cache node_modules directory
          cache(path: 'node_modules', key: 'npm-modules') {
            sh 'npm ci'
          }
        }
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
              // Cache the results directory of OWASP Dependency Check
              cache(path: 'dependency-check-report', key: 'owasp-report') {
                dependencyCheck additionalArguments: ''' 
                    -o './dependency-check-report'
                    -s './'
                    -f 'ALL' 
                    --prettyPrint
                    --nvdApiKey $NVD_API_KEY
                ''', odcInstallation: 'OWASP-DepCheck-10'
              }
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
