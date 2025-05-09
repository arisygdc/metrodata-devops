pipeline {
    agent any
    
    tools{
        maven 'Maven 3.9.5'
        jdk 'JDK 17'
    }
    
    stages {
        stage('Build and test') {

            steps {
                sh './mvnw clean package'
                echo 'Docker Build'
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