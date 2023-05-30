node {
    stage('SCM') {
        checkout scm
    }
    stage('SonarQube analysis') {
        slackSend (channel: 'sonarqube', color: '#0099ff', message: "SONAR ANALYSIS STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
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

                      slackSend (channel: 'sonarqube', color: '#00FF00', message: "SONAR ANALYSIS SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        } else {
            def msg = "${qualitygate.status}: ${env.JOB_NAME} #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

                      slackSend (channel: 'sonarqube', color: '#FF0000', message: "SONAR ANALYSIS FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':\n${sonarqubeURL}")

        }
    }
}
