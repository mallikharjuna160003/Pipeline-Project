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
  stage('Dependency Scanning'){
     parallel {
	   stage("NPM Dependency Audit"){
	      steps {
		sh '''
		    npm audit --audit-level=critical
		    echo $?
		'''
	      }
	   }
	   stage("OWASP Dependency Check"){
	      steps {
		dependencyCheck additionalArguments: ''' 
			    -o './'
			    -s './'
			    -f 'ALL' 
			    --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
		
	      }
	   }
      }
    }
   }    
}
