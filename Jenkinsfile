//BRANCH PIPELINE

pipeline {
  agent { label 'aws-new' }
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
/*
  stages {
    stage('Insert Build Number') {
      steps {
        echo "Some changes for index.html"
        sh'''
	        echo "MAIN $BUILD_NUMBER"
	        sed -i "12 i <p style='text-align:center'>BUILD:$BUILD_NUMBER</p>" index.html
          branchName=$( echo ${GIT_BRANCH#refs/heads/} )
          echo ${GIT_BRANCH#refs/heads/}
          sed -i "13 i <p style='text-align:center'>BRANCH:${branchName}</p>" index.html
          new_title="ENV - ${branchName}"
          sed -i 's/<title>.*</<title>'"${new_title}"'</' index.html
          '''
      }
    }
    stage('HTML test by w3.org validator'){
      steps {
        sh'''
          if curl -H "Content-Type: text/html; charset=utf-8" --data-binary @index.html https://validator.w3.org/nu/?out=json | grep -q error; then
            exit 1
          else
            exit 0
          fi
        '''
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
      steps {
        echo "Build Docker Image"
        script { 
          dockerImage = docker.build "${registryName}:$BUILD_NUMBER"
          dockerImage_l = docker.build "${registryName}:latest" 
        }
      }
    }
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
  }
*/
}
