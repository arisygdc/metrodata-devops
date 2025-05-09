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
                script{
                    env.DATE_TAG = sh(script: "date +%Y-%m-%d", returnStdout: true).trim()
                    sh """
                        docker build --platform=linux/amd64 -t ${REPO_URI}:${env.DATE_TAG} .
                        docker tag ${REPO_URI}:${env.DATE_TAG} ${REPO_URI}:latest
                    """
                }
            }
        }

        stage('Push image') {
            environment {
                ECR_CREDENTIAL = 'ecr:ap-northeast-1:aws-metrodata'
            }

            steps {
                script{
                     docker.withRegistry("${REPO_REGISTRY_URL}", "${ECR_CREDENTIAL}"){
                        echo "Pushing ${REPO_URI}:latest AND ${REPO_URI}:${env.DATE_TAG}"
                        sh "docker push ${REPO_URI}:latest"
                        sh "docker push ${REPO_URI}:${env.DATE_TAG}"
                    }
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