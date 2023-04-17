pipeline {
    agent {
        label 'node-1'
    }
    tools {                       
        maven 'maven 3.8.7'       
    }
    
    stages {
        stage('git') {
            steps {
                git branch: 'declarative', poll: false, url: 'https://github.com/Adithya119/spring-petclinic.git'       /* poll: false --> remote repository will not be polled for changes. */
            }            
        }

        stage('build') {
            steps {
                withSonarQubeEnv(installationName: 'SONAR_9.6.1') {
                    sh 'mvn clean package sonar:sonar'                       
                    }
                }
            }

        stage('QualityGate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
            }
        } 
        }
    }
}


/* Notes :-

By giving tools, you don't need to mention the full path of maven in the below "build" stage 
maven 3.8.7 is the name of the Maven path given in Global tool config 

sonar:sonar is not username:password --> it's a mystery

timeout the project if quality gate doesn't reply within 1 HOUR.
"waitForQualityGate" by giving this, you are telling jenkins to wait for quality gate's input.
"abortPipeline: true" set pipeline to UNSTABLE if quality gate fails 

, envOnly: true, credentialsId: 'SONAR_TOKEN_9.6.1'
*/