pipeline {
    agent any
    
    tools{
        maven 'Maven 3.9.5'
        jdk 'JDK 17'
    }

    environment {
        REPO_URI = "975050033181.dkr.ecr.ap-northeast-1.amazonaws.com/arisy-movie-service"
        REPO_REGISTRY_URL = "975050033181.dkr.ecr.ap-northeast-1.amazonaws.com"
        REGION =  "ap-northeast-1"
    }
    
    stages {
        stage('Compile') {
            steps {
                sh './mvnw clean package'
                echo 'Docker Build'
            }
        }

        stage('Build') {
            steps {
                sh """
                    docker build --platform=linux/amd64 -t ${REPO_URI}:latest .
                """
            }
        }

        stage('Push image') {
            environment {
                ECR_CREDENTIAL = 'ecr:ap-northeast-1:aws-credentials'
            }
            steps {
                docker.withRegistry("${REPO_REGISTRY_URL}", "${ECR_CREDENTIAL}") {
                    sh "docker push ${REPO_URI}:latest"
                }
            }
        }
    }
    post {
        always {
            echo 'This for always notify'
        }
        
        success {
            echo 'Notify success'
        }
        
        failure {
            echo 'Notify Failure'
        }
    }
}