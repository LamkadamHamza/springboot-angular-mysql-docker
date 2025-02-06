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
                    sh "docker build -t lamkadam/angular-app:1.0.0 ."
                    sh "docker tag lamkadam/angular-app:1.0.0 lamkadam/angular-app:latest"
                }                
            }
        }
        stage ("Generate backend image") {
            steps {
                dir("springboot-angular-mysql-docker/springboot/app") {
                    sh "mvn clean install"
                    sh "docker build -t lamkadam/springboot-app:1.0.0 ."
                    sh "docker tag lamkadam/springboot-app:1.0.0 lamkadam/springboot-app:latest"
                }                
            }
        }
        stage ("Push images to Docker Hub") {
            steps {
                sh "docker login -u lamkadam -p 'TON_MOT_DE_PASSE'"
                sh "docker push lamkadam/angular-app:1.0.0"
                sh "docker push lamkadam/springboot-app:1.0.0"
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
