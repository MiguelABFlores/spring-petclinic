pipeline {
    agent any

    // options {
    //     // Add caching for Maven dependencies
    //     cache(name: 'maven', paths: ['~/.m2/repository'])
    // }

    environment {
        M2_HOME = '/opt/apache-maven-3.9.3'
        PATH = "${env.M2_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Setup') {
            steps {
                sh 'echo $PATH'
                sh 'mvn --version'
            }
        }

        stage('Checkstyle') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn checkstyle:checkstyle'
                }
            }
        }

        stage('Test') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn test'
                }
            }
        }

        stage('Build') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    sh 'mvn clean install -Dmaven.test.skip=true'
                }
            }
        }
    }

    post {
        always {
            // Clean up Maven artifacts after each build
            deleteDir()
        }
    }
}
