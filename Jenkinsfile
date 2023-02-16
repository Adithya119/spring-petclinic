/* spc + sonarQube */

pipeline {
    agent { label 'node-1' } 

    stages {
        stage('git') {
            steps { 
                git branch: 'main', url: 'https://github.com/Adithya119/spring-petclinic.git'
            }
        }
        stage('build') {
            steps {   /* installationName refers to name of sonar server configured in Jenkins UI (Manage Jenkins > Configure System); credentialsId is the Id of the User Token using which Jenkins connects with sonar server */
                withSonarQubeEnv(installationName: 'SONAR_9.6', envOnly: true, credentialsId: 'SONAR_TOKEN') { /* refer 'pipeline steps ref' doc */
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