pipeline {
    agent { label 'node-1' } 

    stages {
        stage('git') {
            steps { 
                git branch: 'main', url: 'https://github.com/Adithya119/spring-petclinic.git'
            }
        }
        stage('build') {
            steps {
                withSonarQubeEnv(installationName: 'SONAR_9.6', envOnly: true, credentialsId: 'SONAR_TOKEN') {
                    sh "/usr/local/apache-maven-3.8.7/bin/mvn clean package sonar:sonar"
                    echo "${env.SONAR_HOST_URL}"
            
                }
            }
        }
        stage('archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
        stage('test results') {
            steps {
                junit '**/TEST-*.xml'
            }
        }
    }
    
}