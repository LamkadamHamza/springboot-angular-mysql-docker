pipeline {
    agent any 
    tools { 
        maven 'maven'
        nodejs 'node'
    }
    environment {
        DOCKER_HUB_USER = 'lamkadam'
    }
    stages {
        stage ("Clean up") {
            steps {
                deleteDir()
                sh "docker system prune -af"  
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
                    sh "docker build -t ${DOCKER_HUB_USER}/angular-app:1.0.0 ."
                    sh "docker tag ${DOCKER_HUB_USER}/angular-app:1.0.0 ${DOCKER_HUB_USER}/angular-app:latest"
                }                
            }
        }
        stage ("Generate backend image") {
            steps {
                dir("springboot-angular-mysql-docker/springboot/app") {
                    sh "mvn clean install"
                    sh "docker build -t ${DOCKER_HUB_USER}/springboot-app:1.0.0 ."
                    sh "docker tag ${DOCKER_HUB_USER}/springboot-app:1.0.0 ${DOCKER_HUB_USER}/springboot-app:latest"
                }                
            }
        }
        stage ("Push images to Docker Hub") {
            steps {
               withCredentials([usernamePassword(credentialsId: 'lamkadam', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_HUB_USER --password-stdin"
    sh "docker push \$DOCKER_HUB_USER/angular-app:1.0.0"
    sh "docker push \$DOCKER_HUB_USER/angular-app:latest"
    sh "docker push \$DOCKER_HUB_USER/springboot-app:1.0.0"
    sh "docker push \$DOCKER_HUB_USER/springboot-app:latest"
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
