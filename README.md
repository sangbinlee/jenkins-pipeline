# jenkins-pipeline
jenkins-pipeline




# sudo chmod 666 /var/run/docker.sock

# Installed plugins 


# jenkins-pipeline      버그

    
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







#  https://kanoos-stu.tistory.com/55   참조


        
        jenkins인스턴스 git clone
        jenkins인스턴스 gradle build
        jenkins인스턴스 docker build
        jenkins인스턴스 docker push
        jenkins인스턴스 -> server인스턴스 ssh접속
        server인스턴스 docker pull
        server인스턴스 docker run 
          
  
    
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
  
  







  
  
  
  






# Server인스턴스에 SSH 접속
        ssh agent plugin

        
         pipeline {
            agent any
            stages { 
                stage('SSH SERVER EC2') {
                  steps {
                    echo 'SSH'
                    
                    sshagent(['credentail 식별 값']) {
                        sh 'ssh -o StrictHostKeyChecking=no [user name]@[ip address] "whoami"'
                        sh "ssh -o StrictHostKeyChecking=no [user name]@[ip address] 'docker pull [이미지 이름]:[태그 이름]'"
                        sh "ssh -o StrictHostKeyChecking=no [user name]@[ip address] 'docker run [이미지 이름]:[태그 이름]'"
                    }
                  }
               }
            }
        }





# slack 메세지를 전송

        Dashboard -> Jenkins관리 -> 플러그인 관리 -> slack 검색 및 slack notification 선택 후 설치
        
        pipeline {
            // 스테이지 별로 다른 거
            agent any
        
            stages {
                stage("foo") {
                    steps {
                        echo 'slack test'
                    }
                    post {
                        success {
                            slackSend channel: '#channel-name', color: 'good', message: "success"
                        }
                        failure {
                            slackSend channel: '#channel-name', color: 'danger', message: "failure"
                        }
                    }
                }
            }
        }






# https://myanjini.tistory.com/entry/Jenkins%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Docker-%EB%B9%8C%EB%93%9C%EB%B0%B0%ED%8F%AC 참조



![image](https://github.com/sangbinlee/jenkins-pipeline/assets/4024414/14b1e2f3-bda6-409b-9595-951456e122b8)


 



#
        
        
        pipeline {
            agent any
        
            stages {
                stage('Build') {
                    steps {
                        echo 'Building..'
                    }
                }
                stage('Test') {
                    steps {
                        echo 'Testing..'
                    }
                }
                stage('Deploy') {
                    steps {
                        echo 'Deploying....'
                    }
                }
            }
        }

        
        pipeline {
          agent any
          environment {
            EXAMPLE_KEY = credentials('example-credentials-id') // Secret value is 'sec%ret'
          }
          stages {
            stage('Example') {
              steps {
                  /* CORRECT */
                  bat 'echo %EXAMPLE_KEY%'
              }
            }
          }
        }









        
# https://hyeinisfree.tistory.com/23 참









![image](https://github.com/sangbinlee/jenkins-pipeline/assets/4024414/0340c26b-c458-4175-94d2-1e2a0d4890d4)



# https://fastcampus.co.kr/dev_online_cicd/?utm_source=google&utm_medium=cpc&utm_campaign=hq%5E230725%5E211745&utm_content=jenkins&utm_term=&gclid=CjwKCAiApaarBhB7EiwAYiMwqsSF2TJuYX7-S8Adj0ajRpP3lk-Zw4m5rbMNjR-2jCXbqDqtjaIXkBoCDiMQAvD_BwE



![image](https://github.com/sangbinlee/jenkins-pipeline/assets/4024414/3b524e8b-397b-4ec5-aac5-a8a0503c9c90)


![image](https://github.com/sangbinlee/jenkins-pipeline/assets/4024414/5fce62b1-de0b-410a-b696-e44bbe35ecbd)

![image](https://github.com/sangbinlee/jenkins-pipeline/assets/4024414/4d2e458b-7cbd-43c5-a2e5-aeb5ccb5bfe1)
 



# docker-compose-jenkins.yml


        https://lhamed.github.io/docker-jenkins/ 참조

        sudo docker compose -f docker/docker-compose-jenkins.yml up -d
                
        version: '3.3'
        services:
          jenkins:
            image: jenkins/jenkins:latest
            restart: always
            container_name: "jenkins"
            user: root # volume 을 mount 해서 사용하면, 권한 관련 이슈가 일어나서 추가했습니다. 
            ports:
              - "8080:8080"
            volumes:
              - ${마운트 대상폴더경로를 입력하세요!}:/var/jenkins_home
              - /var/run/docker.sock:/var/run/docker.sock 
            privileged: true  # volume 을 mount 해서 사용하면, 권한 관련 이슈가 일어나서 추가했습니다.         
        






# How do I run a node.js app as a background service?

    https://stackoverflow.com/questions/4018154/how-do-i-run-a-node-js-app-as-a-background-service
    
    
    
    
     
        
        Try to run this command if you are using nohup -
        
        nohup npm start 2>/dev/null 1>/dev/null&
        You can also use forever to start server
        
        forever start -c "npm start" ./ 
        PM2 also supports npm start
        
        pm2 start npm -- start
        
        





#
#
#
# catalog-back.git
        
        pipeline {
            agent any
        
            tools {
                jdk('jdk17')
            }
            stages {
                stage('■■■■■■■■■ ■■■■■■■■■ version') {
                    steps {
                        echo 'Hello World'
                        script {
                        	sh "java -version"
                        }
                    }
                }
                
                stage('■■■■■■■■■ ■■■■■■■■■ clone Prepare') {
                    steps {
                        git branch: 'main', credentialsId: 'sangbinlee',
                            url: 'https://github.com/sangbinlee/catalog-back.git'
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
                
                stage('■■■■■■■■■ ■■■■■■■■■ Build') { 
                    steps {
                    	// gralew이 있어야됨. git clone해서 project를 가져옴.
                        sh 'chmod +x gradlew'
                        //sh  './gradlew clean bootJar'
                        // sh  './gradlew clean build'
                        sh  './gradlew build --warning-mode all'
        
        
                        sh 'ls -al ./build'
                    }
                    post {
                        success {
                            echo '■■■■■■■■■ gradle build success'
                        }
        
                        failure {
                            echo '■■■■■■■■■ gradle build failed'
                        }
                    }
                }
                stage('■■■■■■■■■ ■■■■■■■■■ Test') { 
                    steps {
                        echo  '테스트 단계 xxxxxxxxxxxxxxxxx.'
                    }
                }
                stage('■■■■■■■■■ ■■■■■■■■■ Docker Rm') {
                    steps {
                        sh 'echo "■■■■■■■■■ Docker Rm Start"'
                        sh """
                         docker stop catalog-back
                         docker rm catalog-back
                         docker rmi -f sangbinlee/catalog-back
                        """
                    }
                    
                    post {
                        success { 
                            sh 'echo "Docker Rm Success"'
                        }
                        failure {
                            sh 'echo "Docker Rm Fail"'
                        }
                    }
                }
                
                stage('■■■■■■■■■ ■■■■■■■■■ Dockerizing'){
                    steps{
                        sh 'echo "■■■■■■■■■  Image Bulid Start"'
                        sh 'docker build -t sangbinlee/catalog-back .'
                    }
                    post {
                        success {
                            sh 'echo "Bulid Docker Image Success"'
                        }
        
                        failure {
                            sh 'echo "Bulid Docker Image Fail"'
                        }
                    }
                }
                
                stage('Deploy') {
                    steps {
                        sh 'docker run --name catalog-back -d -p 7080:7080 sangbinlee/catalog-back'
                    }
        
                    post {
                        success {
                            echo 'success'
                        }
        
                        failure {
                            echo 'failed'
                        }
                    }
                }
                
            }
        }
        
        










#




        
        
        
        pipeline {
        
            agent {
                docker { image 'node:20.10.0-alpine3.18' }
            }
            
            // environment {
            //     DOCKER_COMPOSE_VERSION = '1.29.2'
            // }
            stages {
                stage('♥ sample Hello') {
                    steps {
                        echo 'Hello World =  https://www.jenkins.io/doc/book/pipeline/docker/'
                    }
                }
                stage('♥ node version    ') {
                    steps {
                        echo 'node version ..................'
                        script {
                        	sh "node --version"
                        }
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
                        // 	sh "yarn install"
                        	sh "yarn install"
                        }
                    }
                }
                stage('♥ yarn build') {
                    steps {
                        echo '♥ yarn build  ..................'
                        script {
                        // 	sh "yarn build"
                        	sh "npm run build"
                        }
                    }
                }
                stage('♥ yarn --version') {
                    steps {
                        echo '♥ yarn --version  ..................'
                        script {
                        	sh "yarn --version"
                        }
                    }
                }
                // stage('♥ Start Server in Background') {
                //     steps {
                //         echo '♥ Start Server in Background ..................'
                //         script {
                //         // 	sh "npm install pm2 -g"
                //         }
                //     }
                // }
                // stage('♥ yarn start') {
                //     steps {
                //         echo '♥ yarn start  ..................'
                //         script {
                //         	sh "yarn start"
                //         }
                //     }
                // }
                // stage('♥ yarn start') {
                //     steps {
                //         echo '♥ yarn start  ..................'
                //         script {
                //         	sh "yarn start"
                //         }
                //     }
                // }
                // stage('♥ yarn start') {
                //     steps {
                //         echo '♥ yarn start  ..................'
                //         script {
                //         	sh "yarn start"
                //         }
                //     }
                // }
            }
        }
















#

        
        # CONTAINER ID 확인
        sudo docker ps -a
        # log 확인
        sudo docker logs [CONTAINER ID]
        -----------------------------------------
        # 혹은 아래와 같이도 확인가능
        ## 1. 링크된 볼륨
        cat /home/opendocs/jenkins/secrets/initialAdminPassword
        ## 2. 컨테이너 접속해서 확인
        docker exec -it jenkins /bin/bash
        cat /var/jenkins_home/secrets/initialAdminPassword
        






#

        docker run --name nginx -d -p 80:80 --restart always nginx:1.22.1-alpine

#



        
        services:
          nginx:
            container_name: nginx
            image: nginx:1.22.1-alpine
            restart: always
            ports:
              - 80:80
        
                
        docker compose up -d

#
    
        docker compose ps
        docker ps
        
#

        
        docker compose down
        


#

        
        services:
          nginx:
            container_name: nginx
            image: nginx:1.22.1-alpine
            restart: always
            ports:
              - 80:80
            volumes:
              - /etc/letsencrypt:/etc/letsencrypt
              - /var/lib/letsencrypt:/var/lib/letsencrypt
              - ${DOCKER_COMPOSE_ROOT}/nginx/sites.conf:/etc/nginx/conf.d/sites.conf
              - ${DOCKER_COMPOSE_ROOT}/nginx/sites-enabled:/etc/nginx/sites-enabled
        
        
        







#
        
        
        // Nginx 이미지 받기
        $ docker pull nginx
        
        // Nginx 컨테이너 올리기
        $ docker run --name proxy-server -d -p 443:443 -p 80:80 nginx
        
        // 컨테이너 접속
        $ docker exec -it proxy-server bash
        출처: https://lasbe.tistory.com/176 [LasBe's Upgrade:티스토리]









#
#


        
        pipeline {
        
            agent any
            // agent {
            //     docker { image 'node:20.10.0-alpine3.18' }
            // }
            
            // environment {
            //     DOCKER_COMPOSE_VERSION = '1.29.2'
            // }
            stages {
                stage('♥ sample Hello ♥♥♥♥♥♥♥♥♥♥ pwd ') {
                    steps {
                        echo 'Hello World =  https://www.jenkins.io/doc/book/pipeline/docker/'
                        // sh 'apt  install docker-compose'
                        // sh 'docker-compose -version '
                        sh 'pwd '
                    }
                }
                // stage('♥ node version    ') {
                //     steps {
                //         echo 'node version ..................'
                //         script {
                //         	sh "node --version"
                //         }
                //     }
                // }
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
                stage('♥ ll ') {
                    steps {
                        echo '♥ ls -al  ..................'
                        script {
                        	sh "ls -al"
                        }
                    }
                }
                // stage('♥ yarn install ') {
                //     steps {
                //         echo '♥ yarn install  ..................'
                //         script {
                //         	sh "yarn install"
                //         }
                //     }
                // }
                // stage('♥ yarn build') {
                //     steps {
                //         echo '♥ yarn build  ..................'
                //         script {
                //         	sh "yarn build"
                //         // 	sh "npm run build"
                //         }
                //     }
                // }
                // stage('♥ yarn --version') {
                //     steps {
                //         echo '♥ yarn --version  ..................'
                //         script {
                //         	sh "yarn --version"
                //         }
                //     }
                // }
                // stage('♥ Start Server in Background') {
                //     steps {
                //         echo '♥ Start Server in Background ..................'
                //         script {
                //         // 	sh "npm install pm2 -g"
                //         }
                //     }
                // }
                stage('♥ docker build ................') {
                    steps {
                        echo '♥ docker build ..................'
                        script {
                        	sh "docker build -t mini-do:latest ."
                        }
                    }
                }
                stage('♥ ■■■■■■■■■ ■■■■■■■■■ docker images') {
                    steps {
                        echo '♥ ■■■■■■■■■ ■■■■■■■■■ docker images  ..................'
                        script {
                        	sh "docker images"
                        }
                    }
                }
                stage('♥ ■■■■■■■■■ ■■■■■■■■■ docker ps') {
                    steps {
                        echo '♥ ■■■■■■■■■ ■■■■■■■■■ docker ps  ..................'
                        script {
                        	sh "docker ps"
                        }
                    }
                }
                stage('♥ ■■■■■■■■■ ■■■■■■■■■ docker ps -a') {
                    steps {
                        echo '♥ ■■■■■■■■■ ■■■■■■■■■ docker ps -a ..................'
                        script {
                        	sh "docker ps -a"
                        }
                    }
                }
                // stage('♥ ■■■■■■■■■ ■■■■■■■■■ docker run') {
                //     steps {
                //         echo '♥ ■■■■■■■■■ ■■■■■■■■■ docker run  ..................'
                //         // sh 'docker compose down'
                //         // sh 'docker compose up -d'
                //         // script {
                //         // 	sh "docker run mini-do -p 3000:3000 -v /app/node_modules -v .:/app "
                //         // 	sh "docker run mini-do -p 3000:3000"
                //             sh ' docker rm -f mini-do'
                //             sh 'docker run --name mini-do -d -p 3000:3000 mini-do:latest'
                //         // }
                //     }
                // }
                
                
                stage('♥ ■■■■■■■■■ ■■■■■■■■■ docker run     ...........  Deploy............') {
                    steps {
                        sh 'docker rm -f mini-do'
                        sh 'docker run --name mini-do -d -p 3000:3000 mini-do:latest'
                    }
        
                    post {
                        success {
                            echo 'success'
                        }
        
                        failure {
                            echo 'failed'
                        }
                    }
                }
                
                
                
                
                
                // stage('♥ docker-compose --version') {
                //     steps {
                //         echo '♥ docker-compose --version  ..................'
                //         script {
                //         	sh 'sudo curl -sSL "https://github.com/docker/compose/releases/download/v2.15.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                //         	sh "chmod +x /usr/local/bin/docker-compose"
                //         	sh "docker-compose --version"
                //         }
                //     }
                // }
                // stage('♥ yarn start') {
                //     steps {
                //         echo '♥ yarn start  ..................'
                //         script {
                //         	sh "yarn start"
                //         }
                //     }
                // }
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
