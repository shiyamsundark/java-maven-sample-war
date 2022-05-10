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
pipeline{
    agent any
    
    stages{
        stage('Download artifact from nexus'){
            steps{
                sh 'wget --user=admin --password=Athulya@2115 http://20.230.244.137:8081/repository/Nexus-poc/com/github/Premvikash/Example/0.0.1/Example-0.0.1.war'
            }
        }
        stage('Deploy to tomcat'){
            steps{
                sh 'git clone https://github.com/2115-27/java-maven-sample-war'
                sh 'cp java-maven-sample-war/Dockerfile ./'
                sh 'docker build -t tomcat:local'
                sh 'docker run -d --rm -it --name tomcat -p 8888:8080 tomcat:local'
            }
        }
    }
}