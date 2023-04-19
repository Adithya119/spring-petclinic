pipeline {
    agent {
        label 'node-2'
    }

    tools {
        maven 'maven 3.8.8'
    }
    
    triggers {
        upstream(upstreamProjects: 'spc-sonarqube', threshold: hudson.model.Result.SUCCESS)
    }

    stages {
        stage('git') {
            steps {
                git branch: 'artifactory', poll: false, url: 'https://github.com/Adithya119/spring-petclinic.git'
            }
        }

        stage('Artifactory Configuration') {
            steps {
                rtMavenDeployer (                         // setting maven deployer / setting artifactory server
                    id: "Artifactory-1",                  // unique identifier for this server
                    serverId: 'ARTIFACTORY_SERVER',       // referencing the instance_Id in Configure System of Jenkins. 
                    releaseRepo: 'ccp-release',
                    snapshotRepo: 'ccp-snapshot',
                    deployArtifacts: true               // refer notes
                )
            }
        }

        stage('Exec maven / build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'ARTIFACTORY_CREDS', usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {        // creds needed to push build artifacts
                    rtMavenRun (                              // run Artifactory maven / run the goals & push to artifactory server
                        pom: 'pom.xml',                       // path of pom.xml file
                        goals: 'clean package',
                        deployerId: 'Artifactory-1'           // mention this if you have more than 1 artifactory servers configured
                    )
                    
                    stash includes: '**/*.jar', name: 'spc-jar-test'   // stash
                }
            }
        }

        stage('publish build info') {              // pipeline steps ref doc
            steps {
                rtPublishBuildInfo (
                    serverId: 'ARTIFACTORY_SERVER'
                )
            }
        }

        stage('copy jar to k8s master node') {
            agent {                              // different agent used inside a stage
                label 'k8s_master'
            }
            steps {
                unstash 'spc-jar-test'
            }
        }
    }
}


/*

Notes:--

usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD' ----> just give these for anything, as it is - 
doesn't matter what the credentialsId is. They are a mystery


deployArtifacts: true ---> by default, it is false. Give 'true' so that Jenkins pushes build artifacts to the configured artifactory -
server

*/