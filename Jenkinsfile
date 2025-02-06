pipeline {
    agent any
    tools {
        maven 'maven'
        nodejs 'node'
    }
    environment {
        DOCKER_REGISTRY = 'your-docker-registry'
        DOCKER_IMAGE_TAG = 'latest'
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
        stage ("Build and Push Images") {
            parallel {
                stage ("Generate frontend image") {
                    steps {
                        dir("springboot-angular-mysql-docker/angular-app") {
                            sh "docker build -t ${DOCKER_REGISTRY}/angular-app:${DOCKER_IMAGE_TAG} ."
                            sh "docker push ${DOCKER_REGISTRY}/angular-app:${DOCKER_IMAGE_TAG}"
                        }
                    }
                }
                stage ("Generate backend image") {
                    steps {
                        dir("springboot-angular-mysql-docker/springboot/app") {
                            sh "mvn clean install"
                            sh "docker build -t ${DOCKER_REGISTRY}/springboot-app:${DOCKER_IMAGE_TAG} ."
                            sh "docker push ${DOCKER_REGISTRY}/springboot-app:${DOCKER_IMAGE_TAG}"
                        }
                    }
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
    post {
        success {
            echo "Pipeline succeeded!"
            // Add notification or other post-success actions
        }
        failure {
            echo "Pipeline failed!"
            // Add notification or other post-failure actions
        }
    }
}
