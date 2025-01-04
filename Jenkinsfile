@Library('my-shared-library@main') _  // Correct syntax

pipeline {
    agent { label 'slave' }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('checkout') {
            steps {
                script {
                    buildtest.checkoutCode()
                }
            }
        }

        stage('setup java') {
            steps {
                script {
                    buildtest.setupJava17()
                }
            }
        }

        stage('setup mvn') {
            steps {
                script {
                    buildtest.setupMaven()
                }
            }
        }

        stage('setup build') {
            steps {
                script {
                    buildtest.buildProject()
                }
            }
        }

        stage('upload artifact') {
            steps {
                script {
                    buildtest.uploadArtifact('target/*.jar')
                }
            }
        }

        stage('run application') {
            steps {
                script {
                    buildtest.runSpringBootApp()
                }
            }
        }

        stage('validate application') {
            steps {
                script {
                    buildtest.validateAppRunning()
                }
            }
        }

        stage('stop spring') {
            steps {
                script {
                    buildtest.stopSpringBootApp()
                }
            }
        }
    }

    post {
        always {
            script {
                buildtest.cleanupProcesses()
            }
        }
    }
}
