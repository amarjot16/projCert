pipeline {
    agent { label 'webserver' }
    stages {
        stage('clone repo/build-php website') {
	         steps {
                // step1
                echo 'clone......'
                    git url: 'https://github.com/amarjot16/projCert', branch: 'master'
                    sh script: 'cd  $WORKSPACE'
           }		
        }
        
     stage('Push Ansible configuration') {
  	   steps {
               sh  'ansible-playbook ansible_deploy.yml'
	   }
    }
        
      stage('Install Docker-QA') {
  	   steps {
  	             echo 'Install Docker-QA......'
             // sh 'sudo  ansible-playbook Docker_playbook.yml'
	   }
    }
        stage('build & push docker image') {
	         steps {
              withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://index.docker.io/v1/') {
                    sh script: 'docker build -t $JOB_NAME:v1.$BUILD_ID . '
                    sh script: 'docker image tag $JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:v1.$BUILD_ID'
                    sh script: 'docker image tag $JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:latest'
                    sh script: 'docker image push amarjot16/$JOB_NAME:v1.$BUILD_ID'
                    sh script: 'docker push amarjot16/$JOB_NAME:latest'
                    sh script: 'docker image remove $JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:v1.$BUILD_ID amarjot16/$JOB_NAME:latest'
              }	
           }		
        }

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
