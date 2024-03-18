pipeline {
    agent any
    tools {
        maven 'maven 3.9.6'
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
        stage ('Integration testing') {
            steps{
                sh 'mvn verify -DskiUnitTest'
            }
        }
    }
}
