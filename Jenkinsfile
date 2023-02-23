/* spc + sonarQube */

pipeline {
    agent { label 'node-1' }

    tools {    /* by giving tools, you don't need to mention the full path of maven in the below "build" stage */
        maven 'maven 3.8.7'    
    } 

    stages {
        stage('git') {
            steps { 
                git branch: 'main', url: 'https://github.com/Adithya119/spring-petclinic.git'
            }
        }
        stage('build') {
            steps {      /* installationName refers to name of sonar server configured in Jenkins UI (Manage Jenkins > Configure System); credentialsId is the Id of the User Token using which Jenkins connects with sonar server */
                withSonarQubeEnv(installationName: 'SONAR_9.6', envOnly: true, credentialsId: 'SONAR_TOKEN') {    /* refer 'pipeline steps ref' doc */
                    sh "mvn clean package sonar:sonar"
                    timeout(time: 1, unit: 'HOURS') {     /* timeout the project if quality gate doesn't reply within 1 HOUR */
                        waitForQualityGate abortPipeline: true, credentialsId: 'SONAR_TOKEN'
                        /* "waitForQualityGate" by giving this, you are telling jenkins to wait for quality gate's input */
                        /* "abortPipeline: true" set pipeline to UNSTABLE if quality gate fails*/
                    }
            
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