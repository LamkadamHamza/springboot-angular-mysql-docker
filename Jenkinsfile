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
                sh "git clone https://github.com/Raouf-Rhimi/springboot-angular-mysql-docker.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("springboot-angular-mysql-docker/angular-app"){
                    sh "docker build -t angular-app ."
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("springboot-angular-mysql-docker/springboot/app"){
                      sh "mvn clean install"
                      sh "docker build -t springboot-app ."
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("springboot-angular-mysql-docker"){
                    sh " docker compose up -d"
                }                
            }
        }
    }
}
