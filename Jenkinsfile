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
            when {
                branch 'master'
            }
            steps {
                
                withCredentials([usernamePassword(credentialsId: 'webserver_login', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sshPublisher( 
                        alwaysPublishFromMaster: true, 
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging', 
                                transfers: [
                                    sshTransfer(
                                        excludes: '', 
                                        execCommand: 'sudo /usr/bin/systemctl stop train-service && rm -rf /opt/train-service/* && unzip -d /opt/train-service /tmp/trainSchedule.zip && sudo /usr/bin/systemctl start train-service', 
                                        execTimeout: 120000, 
                                        flatten: false, 
                                        makeEmptyDirs: false, 
                                        noDefaultExcludes: false, 
                                        patternSeparator: '[, ]+', 
                                        remoteDirectory: '/tmp', 
                                        remoteDirectorySDF: false, 
                                        removePrefix: 'dist/', 
                                        sourceFiles: 'dist/trainSchedule.zip'
                                    )
                                ], 
                                usePromotionTimestamp: false, 
                                useWorkspaceInPromotion: false, 
                                verbose: true
                            )
                        ]
                    )
                    // sshPublisher(
                    //     failOnError: true,
                    //     continueOnError: false,
                    //     publishers: [
                    //         sshPublisherDesc(
                    //             configName: 'staging',
                    //             sshCredentials: [
                    //                 username: "$USERNAME",
                    //                 encryptedPassphrase: "$PASSWORD"
                    //             ], 
                    //             transfers: [
                    //                 sshTransfer(
                    //                     sourceFiles: 'dist/trainSchedule.zip',
                    //                     removePrefix: 'dist',
                    //                     remoteDirectory: '/tmp',
                    //                     execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && \
                    //                     sudo /usr/bin/systemctl start train-schedule'
                    //                 )
                    //             ]
                    //         )
                    //     ]
                    // )
                }
            }
        }
    }
}