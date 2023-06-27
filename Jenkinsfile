pipeline {
    agent any
    
    stages {
        stage('Merge Request Pipeline') {
            when {
                changeset "origin/${env.BRANCH_NAME}"
            }
            steps {
                // Checkstyle
                sh 'mvn checkstyle:checkstyle'
                archiveArtifacts artifacts: 'target/checkstyle-result.xml', fingerprint: true
                
                // Test
                sh 'mvn test'
                
                // Build without tests
                sh 'mvn package -DskipTests'
                
                // Create Docker image and push to "mr" repository
                sh 'docker build -t spring-petclinic:${GIT_COMMIT:0:7} .'
                sh 'docker tag spring-petclinic:${GIT_COMMIT:0:7} your-repo:8083/mr/spring-petclinic:${GIT_COMMIT:0:7}'
                sh 'docker push your-repo:8083/mr/spring-petclinic:${GIT_COMMIT:0:7}'
            }
        }
        
        stage('Main Branch Pipeline') {
            when {
                branch 'main'
            }
            steps {
                // Create Docker image and push to "main" repository
                sh 'docker build -t spring-petclinic:${GIT_COMMIT:0:7} .'
                sh 'docker tag spring-petclinic:${GIT_COMMIT:0:7} your-repo:8082/main/spring-petclinic:${GIT_COMMIT:0:7}'
                sh 'docker push your-repo:8082/main/spring-petclinic:${GIT_COMMIT:0:7}'
            }
        }
    }
}
