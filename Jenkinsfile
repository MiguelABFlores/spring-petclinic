pipeline {
    agent any

    options {
        // Configure the SCM checkout behavior
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],  // Specify the branch to build
                    userRemoteConfigs: [[url: 'https://github.com/MiguelABFlores/spring-petclinic']],  // Specify the Git repository URL
                    extensions: [
                        [$class: 'CloneOption', depth: 1, noTags: false, reference: '', shallow: true],
                        [$class: 'CleanBeforeCheckout']
                    ]
                ])
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
