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


-----------------
IMPORTANT:--

1) Trainer has removed envOnly: true & credentialsId: 'SONAR_TOKEN_9.6.1' inside withSonarQubeEnv because these were giving error when 
mixed with quality gate. But SONAR_TOKEN has the token so that Jenkins server can connect to Sonar server.

2) Trainer only gave installation name inside withSonarQubeEnv.
withSonarQubeEnv(installationName: 'SONAR_9.6.1') --> by running this, jenkins checks "SonarQube servers" in the configure system -
and this has Sonar server Url & it's Token inside it. This is how jenkins reads & gets sonar's url & token to login to it

3) Configuring a webhook on Sonar server --> thats how sonar server knows the url of Jenkins server (secret is optional here)

Conclusion: --
With the above code (note that Quality Gate is a separate stage) & sonar server's webhook to jenkins server, we are able to play with - 
Quality gate values & fail the build (ERROR: Pipeline aborted due to quality gate failure: ERROR) if code coverage is < 95 %

Note:-
Conditions on "Overall Code" should also be set to fail if coverage is < 95% along with Conditions on "New Code". 
If you only set it for "New code", quality gate is not failing 
*/