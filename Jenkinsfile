//BRANCH PIPELINE

pipeline {
  agent { label 'ubuntu-runner' }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
    disableConcurrentBuilds()
  }

  tools {
	nodejs 'node'
  }

  stages {
	stage('Build') {
		steps {
    			sh'npm install'
      		}
    	}
    	
	stage('Test'){
      		steps {
        		sh'npm test'
      		}
    	}
    	stage('Build Docker image') {
    		steps {
	  	sh '''
			branchName=$( echo ${GIT_BRANCH#refs/heads/} )
			sudo docker build -t "node${branchName}:v1.0" .
	  	'''
		}
    	}

    	stage('Deploy main') {
		when {
        		branch 'main'
    		}
    		steps {
	  		sh '''
				docker ps -aq --filter ancestor=nodemain:v1.0 | xargs docker rm -f
				docker run -d -p 3000:3000 --expose 3000 nodemain:v1.0
			'''
		}
    	}
    	stage('Deploy dev') {
		when {
        		branch 'dev'
    		}
    		steps {
	  		sh '''
				docker ps -aq --filter ancestor=nodedev:v1.0 | xargs docker rm -f
				docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0'
			'''
		}
    	}
  }
}
