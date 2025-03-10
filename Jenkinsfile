pipeline {
  agent any
  tools {
    nodejs 'nodejs-22-6-0'
  }
  environment {
	NVD_API_KEY = 'c89da9be-83e0-4e47-9809-bbe41ef076a7'
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
			    --prettyPrint
                            --nvdApiKey $NVD_API_KEY''', odcInstallation: 'OWASP-DepCheck-10'
		
	      }
	   }
      }
    }
   }    
}
