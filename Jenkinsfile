pipeline {
    agent any
    stages{
        stage('Git checkout') {
            steps{
               git 'https://github.com/swathi-1996/demo-application.git' 
            }
        }
        stage ('unit testing') {
            steps{
                sh 'mvn test'
            }
        }
    }
}
