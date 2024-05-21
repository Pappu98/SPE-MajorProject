pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'souvikiiitb'
        DOCKERHUB_PASSWORD = 'electronicscity@2023'
        IMAGE_NAME_PREFIX = 'souvikiiitb/speProject'
        GITHUB_REPO_URL='https://github.com/Pappu98/SPE-MajorProject.git'
    }

    stages {
        stage('Checkout') {
            steps{
				script{
					git branch: 'master', url: "${GITHUB_REPO_URL}"
				}
            }
        }

        stage('Build and Push Images') {
            parallel {
                stage('Product Service') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-productservice", "./Backend/ProductManagement/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-productservice ${DOCKERHUB_USERNAME}/speProject-productservice:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-productservice:latest"
                        }
                    }
                }

                stage('Cart Service') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-cartservice", "./Backend/ShoppingCartManagement/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-cartservice ${DOCKERHUB_USERNAME}/speProject-cartservice:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-cartservice:latest"
                        }
                    }
                }

                stage('OTP Service') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-otpservice", "./Backend/OtpManagement/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-otpservice ${DOCKERHUB_USERNAME}/speProject-otpservice:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-otpservice:latest"
                        }
                    }
                }

                stage('Login Service') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-loginservice", "./Backend/LoginManagement/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-loginservice ${DOCKERHUB_USERNAME}/speProject-loginservice:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-loginservice:latest"
                        }
                    }
                }

                stage('Order Service') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-orderservice", "./Backend/OrderManagement/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-orderservice ${DOCKERHUB_USERNAME}/speProject-orderservice:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-orderservice:latest"
                        }
                    }
                }

                stage('API Gateway Service') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-apigatewayservice", "./Backend/API-GATEWAY/API-GATEWAY/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-apigatewayservice ${DOCKERHUB_USERNAME}/speProject-apigatewayservice:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-apigatewayservice:latest"
                        }
                    }
                }

                stage('Eureka Server') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-eurekaserver", "./Backend/EurekaServer/EurekaServer/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-eurekaserver ${DOCKERHUB_USERNAME}/speProject-eurekaserver:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-eurekaserver:latest"
                        }
                    }
                }

                stage('Frontend') {
                    steps {
                        script {
                            dockerImage = docker.build("${IMAGE_NAME_PREFIX}-frontend", "./Frontend/amazon-clone/Dockerfile")
                            sh "docker login --username ${DOCKERHUB_USERNAME} --password ${DOCKERHUB_PASSWORD}"
                            sh "docker tag ${IMAGE_NAME_PREFIX}-frontend ${DOCKERHUB_USERNAME}/speProject-frontend:latest"
                            sh "docker push ${DOCKERHUB_USERNAME}/speProject-frontend:latest"
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}