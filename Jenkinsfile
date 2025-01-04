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
                 pipeline.checkoutCode()
                }
            }
        }
        stage('setup java ') {
            steps {
                script {
                 pipeline.setupJava17()
                }
            }
        }
        stage('setup mvn ') {
            steps {
                script {
                 pipeline.setupMaven()
                }
            }
        }  
        stage('setup build ') {
            steps {
                script {
                 pipeline.buildProject()
                }
            }
        }        
        stage('upload artifact ') {
            steps {
                script {
                 pipeline.uploadArtifact('target/*.jar')
                } 
            }
        } 
        stage('run application ') {
            steps {
                script {
                 pipeline.runSpringBootApp()
                }
            }
        } 
        stage('validate application ') {
            steps {
                script {
                 pipeline.validateAppRunning()
                }
            }
        }
        stage('stop spring ') {
            steps {
                script {
                 pipeline.stopSpringBootApp()
                }
            }
        }
    }
post {
        always {
            script {
                 pipeline.cleanupProcesses()
        }
      }
   }
}
