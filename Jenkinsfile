pipeline {
    agent any
    
    environment{
        AUTHOR = "Arisy"
        EMAIL = "arisy@gmail.com"
    }
    
    stages {
        stage('Docker Build') {

            steps {
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