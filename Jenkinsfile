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
        def getURL = readProperties file: './.scannerwork/report-task.txt'
                     sonarqubeURL = "${getURL['dashboardUrl']}"
                                    echo "${sonarqubeURL }"

        if (qualitygate.status == 'SUCCESS') {

            def msg = "${qualitygate.status}: ${env.JOB_NAME} #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

                      slackSend (color: '#FFFF00', message: "BUILD SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        } else {
            def msg = "${qualitygate.status}: ${env.JOB_NAME} #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

                      slackSend (color: '#FFFF00', message: "BUILD FAILED:QUALITY GATE CHECKS FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':\n${sonarqubeURL}")

        }
    }
}
