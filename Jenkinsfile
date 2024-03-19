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
        stage('Artifact uploader'){
            steps {
                script {
                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT")
                            ?"demo-project-snapshot":"demo-project-release"
                    nexusArtifactUploader artifacts:
                     [
                        [
                            artifactId: 'springboot',
                            classifier: '',
                            file: 'target/Uber.jar',
                            type: 'jar'
                        ]
                      ],
                          credentialsId: 'nexus-api',
                          groupId: 'com.example', 
                          nexusUrl: '13.232.231.20:8081', 
                          nexusVersion: 'nexus3', 
                          protocol: 'http', 
                          repository: "$nexusRepo", 
                          version: "$readPomVersion.version"
                }
            }
        } 
        stage ('Docker Image Build') {
            steps {
                script {
                    sh 'docker image build -t $JOB_NAME:V1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:V1.$BUILD_ID ruttalaswathi/$JOB_NAME:V1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:V1.$BUILD_ID ruttalaswathi/$JOB_NAME:latest' 
                }
            }
        } 
         stage ('Push Docker Image') {
            steps {
                script {
                    withCredentials([
                        usernameColonPassword(
                            credentialsId: 'dockerhub-cred', 
                            variable: 'dockerhub-cred')]) {
                                sh 'docker login -u ruttalaswathi -p ${dockerhub-cred}'
                                sh 'docker image push ruttalaswathi/$JOB_NAME:V1.$BUILD_ID'
                                sh 'docker image push ruttalaswathi/$JOB_NAME:latest'
                    }
                }
            }
        } 
        }

    }
