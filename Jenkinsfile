pipeline {
    agent {
        docker {
            image 'maven:latest'
            runArgs '-v /your/local/maven/repository:/root/.m2/repository'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the 'main' branch from Git
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/MiguelABFlores/spring-petclinic']]])
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
