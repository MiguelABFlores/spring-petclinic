pipeline {
    agent any

    stages {
        stage('Setup') {
            steps {
                sh 'printenv'
            }
        }

        stage('Checkstyle') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
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
}
