node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube analysis') {
  def scannerHome = tool 'sonar-scanner';
  withSonarQubeEnv('sonardemo') {
    sh "${scannerHome}/bin/sonar-scanner"
  }
}
stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }        
}