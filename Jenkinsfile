// This code is just to build the code & push artifacts to Artifactory (jfrog)

// for explanation of all fields, refer your notes

pipeline {
    agent { label 'node-1' } 

    stages {
        stage('git') {
            steps { 
                git branch: 'stash-unstash', url: 'https://github.com/Adithya119/spring-petclinic.git'
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
                        tool: 'maven 3.8.7', // Maven installation name from Global tools configuration // copied from "tools" stage
                        pom: 'pom.xml',
                        goals: 'clean install',
                        deployerId: "MAVEN_DEPLOYER",
                    )
                    stash includes: '**/*.jar', name: 'spcjar'
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

        stage ('copy to other node') {
            agent { label 'master'}
            steps {
                unstash 'spcjar'
            }
        }

    }
    
}