//BRANCH PIPELINE

pipeline {
  agent { label 'internal' }
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

  stages {
    stage('npm install') {
      steps {
//        echo "Some changes for index.html"
        sh'npm install'
      }
    }
    stage('npm test'){
      steps {
        sh'npm test'
      }
    }
    stage('Install and run Docker') {
      steps {
        sh '''
            sudo yum install docker -y
            sudo systemctl start docker
            sudo usermod -aG docker $USER
            sudo chmod 666 /var/run/docker.sock
            sudo systemctl restart docker

        '''
      }
    }
    stage('Build Docker image') {
	when {
        	branch 'main'
    	}
    	steps {
	  sh 'docker build -t nodemain:v1.0 for main'
	}
	when {
        	branch 'dev'
    	}
    	steps {
	  sh 'docker build -t nodedev:v1.0 for main'
	}
    }
/*
    stage('Upload to DockerHub') {
      steps {
        echo "Upload to DockerHub"
        script { 
          docker.withRegistry( '', registryCredential ) { 
                dockerImage.push()
                dockerImage_l.push()
          }				
        }
      slackSend color: "good", message: "CI pipeline successfully finished"
      }
    }
*/
  }
}
