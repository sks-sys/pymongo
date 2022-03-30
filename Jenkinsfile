pipeline {
    agent any 
    environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "your_docker_user_id/mypythonapp"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'docker_id'
        dockerImage = ''
    }
    
    stages {
        stage('Cloning Git') {
            steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sks-sys/pymongo.git']]])
       
            }
        }
    
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
    
    stage('publish') {
        steps {
         echo 'tested successfully!!'
        }
    }
    }
}


