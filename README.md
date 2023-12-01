# jenkins-pipeline
jenkins-pipeline




#

# Installed plugins 


# jenkins-pipeline

    
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
  
  
    
    pipeline {
        agent any
    
        triggers {
            pollSCM('*/3 * * * *')
        }
    
        environment {
            imagename = "docker build로 만들 이미지 이름"
            registryCredential = 'docker hub credential ID'
            dockerImage = ''
        }
    
        stages {
            // git에서 repository clone
            stage('Prepare') {
              steps {
                echo 'Clonning Repository'
                git url: 'git repository 주소',
                  branch: 'master',
                  credentialsId: '생성한 github access token credentail id'
                }
                post {
                 success { 
                   echo 'Successfully Cloned Repository'
                 }
               	 failure {
                   error 'This pipeline stops here...'
                 }
              }
            }
    
            // gradle build
            stage('Bulid Gradle') {
              agent any
              steps {
                echo 'Bulid Gradle'
                dir ('.'){
                    sh """
                    ./gradlew clean build --exclude-task test
                    """
                }
              }
              post {
                failure {
                  error 'This pipeline stops here...'
                }
              }
            }
            
            // docker build
            stage('Bulid Docker') {
              agent any
              steps {
                echo 'Bulid Docker'
                script {
                    dockerImage = docker.build imagename
                }
              }
              post {
                failure {
                  error 'This pipeline stops here...'
                }
              }
            }
    
            // docker push
            stage('Push Docker') {
              agent any
              steps {
                echo 'Push Docker'
                script {
                    docker.withRegistry( '', registryCredential) {
                        dockerImage.push("docker tag name")  // ex) "1.0"
                    }
                }
              }
              post {
                failure {
                  error 'This pipeline stops here...'
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
