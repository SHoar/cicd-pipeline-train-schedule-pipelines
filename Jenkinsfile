pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sshPublisher(publishers: [
                        sshPublisherDesc(configName: 'staging', transfers: [
                            sshTransfer(excludes: '', execCommand: 'systemctl stop train-schedule && systemctl start train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/opt/train-schedule', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)
                            ]
                    )

                }
            }
        }
    }
}