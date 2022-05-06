pipeline {
    agent any
    tools{
        maven 'MAVEN'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'mvn clean verify'
            }
        }
        stage('Run'){
            steps{
                echo 'Build successful'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('Sonarqube') {
                    
                    withMaven(maven:'MAVEN') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage('Upload war to nexus'){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'Example',
                         classifier: '', 
                         file: 'target/Example-0.0.1.war',
                          type: 'war'
                    ]
                ],
                 credentialsId: 'nexus-cred',
                  groupId: 'com.github.Premvikash', 
                  nexusUrl: '20.230.244.137:8081', 
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  repository: 'Nexus-poc',
                  version: '0.0.1'
            }
        }
    }
}