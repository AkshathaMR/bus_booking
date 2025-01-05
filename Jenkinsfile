@Library('my-shared-library@main') _  // Correct syntax

pipeline {
    agent { label 'slave' }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64'
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
		SONAR_TOKEN = credentials('SONAR_TOKEN') 
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
		//add your own sonar account details  
        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=akshatha111_bus-booking \
                      -Dsonar.organization=akshatha111 \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
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
