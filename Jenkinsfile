pipeline {
    agent any
    stages {
        stage('clone repo/build-php website') {
	         steps {
                // step1
                echo 'clone......'
                    git url: 'https://github.com/amarjot16/projCert', branch: 'master'
                    sh script: 'cd  $WORKSPACE'
		            //sh script: 'cp -R * /opt/docker/'	
           }		
        }
        stage('build & push docker image') {
	         steps {
              withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://index.docker.io/v1/') {
                  //  sh script: 'cd  /opt/docker'
                    sh script: 'docker build -t $JOB_NAME:v1.$BUILD_ID . '
                    sh script: 'docker image tag $JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:v1.$BUILD_ID'
                    sh script: 'docker image tag $JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:latest'
                    sh script: 'docker image push amarjot16/$JOB_NAME:v1.$BUILD_ID'
                    sh script: 'docker push amarjot16/$JOB_NAME:latest'
                    sh script: 'docker image remove $JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:latest'
              }	
           }		
        }
    //stage('Install Docker-QA') {
  	 //  steps {
     //         sh 'sudo -u devops ansible-playbook /opt/docker/Docker_playbook.yml'
	 //  }
   // }
    stage('Deploy-App-QA') {
  	   steps {
              sh 'ansible-playbook ansible_deploy.yml'
	   }
	   //post { 
           //   always { 
           //     cleanWs() 
	   //   }
	   //}
	}
}
}
