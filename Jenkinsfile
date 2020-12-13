pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './gradlew build'
                archiveArtifacts artifact: 'dist/cicd-pipeline-trains.zip'
            }
        }
    }
}