pipeline {
    agent any

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
                    withMaven(maven: 'Maven 3.9.3') {
                        sh 'mvn checkstyle:checkstyle'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    withMaven(maven: 'Maven 3.9.3') {
                        sh 'mvn test'
                    }
                }
            }
        }

        // stage('Build') {
        //     steps {
        //         // Build the project without tests using Maven or Gradle
        //     }
        // }

        // stage('Create Docker Image - MR') {
        //     when {
        //         branch 'merge_request/*'
        //     }
        //     steps {
        //         // Create a Docker image using the Dockerfile in the repository
        //         // Tag the image with GIT_COMMIT (short)
        //         // Push the image to the "mr" repository in Nexus or Docker Hub
        //     }
        // }

        // stage('Create Docker Image - Main') {
        //     when {
        //         branch 'main'
        //     }
        //     steps {
        //         // Create a Docker image using the Dockerfile in the repository
        //         // Push the image to the "main" repository in Nexus or Docker Hub
        //     }
        // }
    }

    post {
        always {
            // Clean up Maven artifacts after each build
            deleteDir()
        }
    }
}
