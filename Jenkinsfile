pipeline {
    agent any 
    tools { 
        maven 'maven'
        nodejs 'node'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                bat "git clone https://github.com/Raouf-Rhimi/springboot-angular-mysql-docker.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("springboot-angular-mysql-docker/angular-app"){
                    bat "docker build -t angular-app ."
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("springboot-angular-mysql-docker/springboot/app"){
                      bat "mvn clean install"
                      bat "docker build -t springboot-app ."
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("springboot-angular-mysql-docker"){
                    bat " docker compose up -d"
                }                
            }
        }
    }
}
