node('node-1') {
    stage('git') {
      git branch: 'main', url: 'https://github.com/Adithya119/spring-petclinic.git'
    stage('build') {
      sh '''
          echo $PATH
          echo $M2_HOME
          echo $JAVA_HOME
         '''
    stage('arhcive') {
      archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
    stage('publish test results') {
      junit '**/TEST-*.xml'
}
}
}
}
}
