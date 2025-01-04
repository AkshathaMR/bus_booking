@Library('my-shared-library@main') _  // Correct syntax

pipeline {
    agent { label 'slave' }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    pipeline.checkoutCode()
                }
            }
        }

        stage('Setup Java') {
            steps {
                script {
                    pipeline.setupJava17()
                }
            }
        }

        stage('Setup Maven') {
            steps {
                script {
                    pipeline.setupMaven()
                }
            }
        }

        stage('Build Project') {
            steps {
                script {
                    pipeline.buildProject()
                }
            }
        }

        stage('Upload Artifact') {
            steps {
                script {
                    pipeline.uploadArtifact('target/*.jar')
                }
            }
        }

        stage('Run Application') {
            steps {
                script {
                    pipeline.runSpringBootApp()
                }
            }
        }

        stage('Validate Application') {
            steps {
                script {
                    pipeline.validateAppRunning()
                }
            }
        }

    }
}
