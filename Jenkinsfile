node {
    stage('SCM') {
        checkout scm
    }
    stage('SonarQube analysis') {
        slackSend (color: '#FFFF00', message: "BUILD STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        def scannerHome = tool 'sonar-scanner';
        withSonarQubeEnv('sonardemo') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }

    stage("Quality Gate") {
        qualitygate = waitForQualityGate()
        if (qualitygate.status!= "OK") {
            currentBuild.result = "FAILURE"
        }
        def color

        if (qualitygate.status == 'STARTED') {
            color = '#D4DADF'
        } else if (qualitygate.status == 'SUCCESS') {
            color = '#BDFFC3'
        } else if (qualitygate.status == 'UNSTABLE') {
            color = '#FFFE89'
        } else {
            color = '#FF9FA1'
        }

        def msg = "${qualitygate.status}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

                  slackSend(color: color, message: msg)
    }
}
