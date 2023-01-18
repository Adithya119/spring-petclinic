node('node-1') {
    stage('git') {
      git branch: 'main', url: 'https://github.com/Adithya119/spring-petclinic.git'
    stage('build') {
      sh 'mvn clean package'
    stage('arhcive') {
      archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
    stage('publish test results') {
      junit '**/TEST-*.xml'
}
}
}
}
}
