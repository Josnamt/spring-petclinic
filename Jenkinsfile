@Library('jenk_lib') _

pipeline {
    agent {
        docker {
            image 'maven:3.9.6-eclipse-temurin-21'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
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
                script {
                    buildStages.buildAndTest()
                }
            }
        }

        stage('Publish Test Results') {
            steps {
                script {
                    buildStages.publishTestResults()
                }
            }
        }

        stage('Publish Artifacts') {
            steps {
                script {
                    buildStages.publishArtifacts()
                }
            }
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
