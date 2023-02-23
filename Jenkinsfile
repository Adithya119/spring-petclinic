// This code is just to build the code & push artifacts to Artifactory (jfrog)

pipeline {
    agent { label 'node-1' } 

    tools {    /* by giving tools, you don't need to mention the full path of maven in the below "build" stage */
        maven 'maven 3.8.7'    
    }

    stages {
        stage('git') {
            steps { 
                git branch: 'jfrog', url: 'https://github.com/Adithya119/spring-petclinic.git'
            }
        }        
        
        stage ('Artifactory configuration') {  /* this is to push artifacts to Artifactory */
            steps {
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: 'JFROG-OSS',
                    releaseRepo: 'ccp-releases',
                    snapshotRepo: 'ccp-snapshots'
                )

            }
        }
    
        stage ('Exec Maven') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'ARTIFACTORY_PASSWORD', usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
                    rtMavenRun (
                        tool: 'maven 3.8.7', // Tool name from Jenkins configuration // copied from "tools" stage
                        pom: 'pom.xml',
                        goals: 'clean install',
                        deployerId: "MAVEN_DEPLOYER",
                    )
                }
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: 'JFROG-OSS'
                )
            }
        }

    }
    
}