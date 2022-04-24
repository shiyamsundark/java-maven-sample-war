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
    }
}