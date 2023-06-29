pipeline {
    agent any

    environment {
        M2_HOME = '/opt/apache-maven-3.6.3'
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
