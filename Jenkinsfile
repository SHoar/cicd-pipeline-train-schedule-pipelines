pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './gradlew build --no-daemon'
                sh 'ls -al'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    }
}