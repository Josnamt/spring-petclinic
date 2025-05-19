@Library('jenk_lib') _

pipeline {
    agent any

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
                    docker.image('maven:3.9.6-eclipse-temurin-21').inside('-v /var/run/docker.sock:/var/run/docker.sock') {
                        buildStages.buildAndTest()
                    }
                }
            }
        }

        stage('Publish Test Results') {
            steps {
                script {
                    docker.image('maven:3.9.6-eclipse-temurin-21').inside('-v /var/run/docker.sock:/var/run/docker.sock') {
                        buildStages.publishTestResults()
                    }
                }
            }
        }

        stage('Publish Artifacts') {
            steps {
                script {
                    docker.image('maven:3.9.6-eclipse-temurin-21').inside('-v /var/run/docker.sock:/var/run/docker.sock') {
                        buildStages.publishArtifacts()
                    }
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
