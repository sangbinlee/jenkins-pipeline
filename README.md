# jenkins-pipeline
jenkins-pipeline




#

  
  pipeline {
  
      agent {
          docker { image 'node:20.10.0-alpine3.18' }
      }
      
      // environment {
      //     DOCKER_COMPOSE_VERSION = '1.29.2'
      // }
      stages {
          stage('Hello') {
              steps {
                  echo 'Hello World'
              }
          }
          stage('■■■■■■■■■ ■■■■■■■■■ clone Prepare') {
              steps {
                  git branch: 'main', credentialsId: 'mini-do',
                      url: 'https://github.com/sangbinlee/mini-do.git'
              }
              
              post {
                  success { 
                      sh 'echo "Successfully Cloned Repository"'
                  }
                  failure {
                      sh 'echo "Fail Cloned Repository"'
                  }
              }    
          }
          stage('♥ node version') {
              steps {
                  echo 'node version ..................'
                  script {
                  	sh "node --version"
                  }
              }
          }
          stage('♥ ll ') {
              steps {
                  echo '♥ ls -al  ..................'
                  script {
                  	sh "ls -al"
                  }
              }
          }
          stage('♥ yarn install ') {
              steps {
                  echo '♥ yarn install  ..................'
                  script {
                  	sh "yarn install"
                  }
              }
          }
          stage('♥ yarn build') {
              steps {
                  echo '♥ yarn build  ..................'
                  script {
                  	sh "yarn build"
                  }
              }
          }
          stage('♥ yarn start') {
              steps {
                  echo '♥ yarn start  ..................'
                  script {
                  	sh "yarn start"
                  }
              }
          }
      }
  }







#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
#
