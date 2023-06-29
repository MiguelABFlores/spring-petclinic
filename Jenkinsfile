pipeline {
    agent {
        dockerContainer {
            image 'maven:latest'
            args '-v /your/local/maven/repository:/root/.m2/repository'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from Git
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Run Maven build
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                // Run Maven tests
                sh 'mvn test'
            }
        }
        // Add more stages as needed
    }
}
