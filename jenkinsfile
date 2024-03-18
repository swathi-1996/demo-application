pipeline {
    agent any
    tools {
        maven 'Maven 3.9.6'
    }
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
