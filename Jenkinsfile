@Library('my-shared-library@main') _

pipeline {
    agent { label 'slave' }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('pipeline') {
            steps {
                pipeline() // Assumes this is defined in the shared library
            }
        }
    }

    post {
        always {
            cleanup() // Assumes this is defined in the shared library
        }
    }
}
