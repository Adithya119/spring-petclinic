pipeline {
    agent {
        label 'node-1'
    }
    tools {                       /* by giving tools, you don't need to mention the full path of maven in the below "build" stage */
        maven 'maven 3.8.7'       /* maven 3.8.7 is the name of the Maven path given in Global tool config */
    }
    
    stages {
        stage('git') {
            steps {
                git branch: 'declarative', poll: false, url: 'https://github.com/Adithya119/spring-petclinic.git'       /* If poll is false, then the remote repository will not be polled for changes. */
            }            
        }

        stage('build') {
            steps {
                withSonarQubeEnv(installationName: 'SONAR_9.6.1', envOnly: true, credentialsId: 'SONAR_TOKEN_9.6.1') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
}