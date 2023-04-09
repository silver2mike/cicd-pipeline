//BRANCH PIPELINE

pipeline {
  agent { label 'ubuntu' }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
    disableConcurrentBuilds()
  }

   environment { 

        registryCredential  = 'dockerhub_mikedzn' 
        dockerImage         = '' 
        dockerImage_l       = ''
        registryName        = "mikedzn/epam_${env.BRANCH_NAME}"
   }

  tools {
	nodejs 'node'
  }

  stages {
    stage('Build') {
      steps {
//        echo "Some changes for index.html"
        sh'npm install'
      }
    }
    stage('Test'){
      steps {
        sh'npm test'
      }
    }
    stage('Install Docker') {
      steps {
	echo ""
/*
        sh '''

		sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
		curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
		echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
		sudo apt update -y
		apt-cache policy docker-ce
		sudo apt install -y docker-ce
        '''
*/
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
	  sh 'sudo docker run -d -p 3000:3000 --expose 3000 nodemain:v1.0'
	}
    }
    stage('Deploy main') {
	when {
        	branch 'dev'
    	}
    	steps {
	  sh 'sudo docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0'
	}
    }

  }
}
