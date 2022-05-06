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
                script{
                    def mavenPom = readMavenPom file: 'pom.xml'
                     nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'Example',
                         classifier: '', 
                         file: "target/Example-${mavenPom.version}.war",
                          type: 'war' 
                    ]
                ],
                 credentialsId: 'nexus-cred',
                  groupId: 'com.github.Premvikash', 
                  nexusUrl: '20.230.244.137:8081', 
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  repository: 'Nexus-poc',
                  version: "${mavenPom.version}"
                }

               
            }
        }
    }
}