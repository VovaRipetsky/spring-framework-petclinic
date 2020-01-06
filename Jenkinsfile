pipeline {
        agent none
        tools
             {
       		  git 'Default'
       		  maven 'maven'
             }
              stages{
             	                      
                     stage('Clone Git WAR'){
                     agent { label 'master' }
                    	steps{
                               git branch: 'master', url: 'https://github.com/VovaRipetsky/spring-framework-petclinic.git'
                             }
                      }
              	     stage('Build&Test WAR'){
                     agent { label 'master' }
                         steps{
                    		sh 'mvn clean package -P MySQL'
                              }
                      }
                      stage('Deploy WAR'){
                      agent { label 'master' }
                         steps{
                        sh 'scp -i /var/lib/jenkins/.ssh/id_rsa -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipe_mvn_war/target/*.war ubuntu@172.31.24.91:/home/ubuntu/docker-composes/tomcat'

                              }
                      }
                      
                      
                      stage('DockerCompose'){
                      agent { label 'docker_slave' }
                         steps{
                                sh 'docker-compose -f /home/ubuntu/docker-composes/tomcat/docker-compose-tomcat.yml up -d'
                                sh 'docker-compose -f /home/ubuntu/docker-composes/tomcat/docker-compose-tomcat.yml restart'
                              }
                      }
                      		
                     }
           }
