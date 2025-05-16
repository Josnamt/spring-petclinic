@Library('jenk_lib') _

pipeline{
    agent any

    tools {
        maven 'maven-local'
        jdk 'jdk-21-local'
    }

    environment {
        ARTIFACTORY_SERVER = 'artifactory-prod'
        ARTIFACTORY_REPO = 'libs-release-local'
    }
    options {
        ansiColor('xterm')
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                buildStages.buildAndTest()
            }
        }

        stage('Publish Test Results') {
            steps {
                buildStages.publishTestResults()
            }
        }

        stage ('publish Artifacts'){
            steps{
                buildStages.publishArtifacts()
            }
        }
        post {
            success {
                echo "Build success"
            }
            failure {
                echo "Build failed"
            }
        }
    }
}