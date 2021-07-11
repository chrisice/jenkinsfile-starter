env.niceName = 'Build Name'

pipeline {
    agent any
    //agent { label "jenkins-node-1"}
    triggers {
        cron ()
    }
    stages {
        stage ('Stage Name') {
            steps {
                sh 'echo "Hello World"'
            }
        }
    }
    post {
        always {
            cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable:true, cleanupMatrixParent: true, deleteDirs: true)
        }
        success {
            slackSend(color: 'good', message: "*${niceName}* pipeline has finished build ${currentBuild.displayName} :white_check_mark:\nResult: ${currentBuild.currentResult}\nRun Duration: ${currentBuild.durationString}\n<${RUN_DISPLAY_URL}|View Build (#${currentBuild.number})> - <${JOB_DISPLAY_URL}|View All Builds>");
        }
        failure {
            slackSend(channel: "#alert", color: 'danger', message: "*${niceName}* pipeline has failed build ${currentBuild.displayName} :x:\n<${RUN_DISPLAY_URL}|View Build (#${currentBuild.number})> - <${JOB_DISPLAY_URL}|View All Builds>");
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n\nMore info at: ${RUN_DISPLAY_URL}",
            recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
            subject: "Jenkins Build ${currentBuild.currentResult}: Build ${env.BUILD_NUMBER} - Job ${env.JOB_NAME}"
        }
        unstable {
            slackSend(color: 'warning', message: "*${niceName}* pipeline has been marked as unstable after build ${currentBuild.displayName}\n<${RUN_DISPLAY_URL}|View Build (#${currentBuild.number})> - <${JOB_DISPLAY_URL}|View All Builds>");
        }
    }
}
