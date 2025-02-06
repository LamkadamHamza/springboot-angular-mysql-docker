pipeline {
    agent any
    tools {
        maven 'maven'
        nodejs 'node'
    }
    stages {
        stage ("Clean up") {
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo") {
            steps {
                sh "git clone https://github.com/LamkadamHamza/springboot-angular-mysql-docker.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                dir("springboot-angular-mysql-docker/angular-app") {
                    sh "docker build -t angular-app ."
                    sh "docker tag angular-app malbouz/angular-app:1.0.0"
                }
            }
        }
        stage ("Generate backend image") {
            steps {
                dir("springboot-angular-mysql-docker/springboot-app") { // Vérifie le bon chemin !
                    sh "mvn clean install"
                    sh "docker build -t springboot-app ."
                    sh "docker tag springboot-app malbouz/springboot-app:1.0.0"
                }
            }
        }
        stage ("Run docker compose") {
            steps {
                dir("springboot-angular-mysql-docker") {
                    sh "docker compose up -d"
                }
            }
        }
    }
}
