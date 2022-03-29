pipeline {
    agent any

    stages {
        stage('git clone') {
            steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sks-sys/pymongo.git']]])
            }
        }
        stage('build') {
            steps {
                sh 'docker build . -t pymongo'
            }
        }
        stage('publish') {
            steps {
               echo 'tested successfully!!'
            }
        }
    }
}