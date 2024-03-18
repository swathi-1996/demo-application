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
        stage ('maven build') {
            steps{
                sh 'mvn clean install'
            }
        }
        stage ('static code analysis') {
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                       sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage ('Quality Gate status') {
            steps{
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
        }

    }
}
